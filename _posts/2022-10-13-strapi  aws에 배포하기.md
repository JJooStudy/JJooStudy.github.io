---
layout: post
title:  "strapi - aws에 배포하기"
author: "JJoo"
comments: true
tags: strapi
---


## local에서 작업한 strapi를 aws에 배포해보자

strapi에서 제공하는 문서를 토대로 작업했다. 

[strapi aws 배포](https://docs.strapi.io/developer-docs/latest/setup-deployment-guides/deployment/hosting-guides/amazon-aws.html)

서버에 git과 nodejs가 모두 구성되어 있고 strapi 프로젝트가 github에 올라가있는 상황에서부터 시작하는 글이다. 

strapi 프로젝트 구현은 아래 게시글을 참고

[strapi 시작하기](https://jjoostudy.github.io/2022-08-30/strapi-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)

[strapi api 사용하기](https://jjoostudy.github.io/2022-09-14/strapi-api-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)


## 서버에 strapi 프로젝트 올리기 

### 1. pg 패키지 설치 

postgreSQL을 데이터베이스로 사용하기 때문에 pg 라는 패키지를 설치해준다.

> postgreSQL란?
> 
> 오픈 소스 객체-관계형 데이터베이스 시스템(ORDBMS)으로 다른 관계형 데이터베이스 시스템과 달리 다양한 데이터베이스 객체를 사용자가 만들 수 있는 기능을 제공한다. 
> 
> 오라클 개발자들이 같이 개발해 오라클과 유사하다. 
> 
> 관계형 데이터베이스 시스템(RDBMS)과의 차이점은 데이터의 저장 및 접근 방법에 대한 관점의 차이다. 
> 
> RDBMS는 행과 열이 있는 하나 이상의 관계 또는 테이블의 모음, ORDBMS는 데이터가 객체로 저장된 것처럼 작동한다.


``` npm install pg ```

pg 설치 후 ``` ./config/database.js ``` 파일을 아래처럼 수정한다. 

```javascript 
module.exports = ({ env }) => ({
  connection: {
    client: "postgres",
    connection: {
      host: env("DATABASE_HOST", "127.0.0.1"),
      port: env.int("DATABASE_PORT", 5432),
      database: env("DATABASE_NAME", "strapi"),
      user: env("DATABASE_USERNAME", ""),
      password: env("DATABASE_PASSWORD", ""),
    },
    useNullAsDefault: true,
  },
});
```

### 2. Strapi AWS S3 Upload Provider 설치 

로컬서버 대신 aws s3에 업로드 할 수 있게 provider를 설치해준다. 

``` npm install @strapi/provider-upload-aws-s3 ```

설치 후 ``` ./config/plugins.js ``` 파일을 생성해 아래 내용을 넣어준다. 

```javascript 
module.exports = ({ env }) => ({
  upload: {
    config: {
      provider: 'aws-s3',
      providerOptions: {
        accessKeyId: env('AWS_ACCESS_KEY_ID'),
        secretAccessKey: env('AWS_ACCESS_SECRET'),
        region: env('AWS_REGION'),
        params: {
            Bucket: env('AWS_BUCKET_NAME'),
        },
      },
      // These parameters could solve issues with ACL public-read access — see [this issue](https://github.com/strapi/strapi/issues/5868) for details
      actionOptions: {
        upload: {
          ACL: null
        },
        uploadStream: {
          ACL: null
        },
      }
    },
  }
});
```

### 3.github에서 EC2 인스턴스에 배포

위 내용까지 반영된 git repo를 서버에 배포한다.

PuTTY로 aws 서버에 붙는 다. 

[PuTTY 설정하기](https://jjoostudy.github.io/2022-10-13/PuTTY-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

git에 올린 strapi 프로젝트를 서버에 clone 받는 다.

``` git clone https://github.com/your-name/내프로젝트repo.git ```

그 다음 strapi 프로젝트 루트 폴더로 이동 후 npm 패키지들을 설치한 후 실행해준다. 

```
cd ./my-project/
npm install
NODE_ENV=production npm run build
```

### 4. pm2 runtime 설치 

pm2란 ? 

Process Manager의 약어이고,

프로세스들이 계속 실행될 수 있는 환경을 제공해주는 등 node.js 로 만들어진 앱을 쉽게 관리할 수 있게 해준다.

pm2 runtime을 사용해서 strapi 프로젝트를 유지하고 downtime 없이 reload 할 수 있다. 

아래 명령어로 pm2를 전역으로 설치한다. 

``` npm install pm2@latest -g ```

설치 후 ``` ecosystem.config.js ``` 파일을 구성해준다. 

nano editer로 열고 내용을 편집한다. 

```
cd ~
pm2 init
sudo nano ecosystem.config.js
```

아래 내용을 넣어준다. 

```javascript 
module.exports = {
  apps: [
    {
      name: 'your-app-name', // Your project name
      cwd: '/home/ubuntu/my-project', // Path to your project
      script: 'npm', // For this example we're using npm, could also be yarn
      args: 'start', // Script to start the Strapi server, `start` by default
      env: {
        APP_KEYS: 'your app keys', // you can find it in your project .env file.
        API_TOKEN_SALT: 'your api token salt',
        ADMIN_JWT_SECRET: 'your admin jwt secret',
        JWT_SECRET: 'your jwt secret',
        NODE_ENV: 'production',
        DATABASE_HOST: 'your-unique-url.rds.amazonaws.com', // database Endpoint under 'Connectivity & Security' tab
        DATABASE_PORT: '5432',
        DATABASE_NAME: 'strapi', // DB name under 'Configuration' tab
        DATABASE_USERNAME: 'postgres', // default username
        DATABASE_PASSWORD: 'Password',
        AWS_ACCESS_KEY_ID: 'aws-access-key-id',
        AWS_ACCESS_SECRET: 'aws-access-secret', // Find it in Amazon S3 Dashboard
        AWS_REGION: 'aws-region',
        AWS_BUCKET_NAME: 'my-project-bucket-name',
      },
    },
  ],
};
```

위 내용을 ```.env``` 파일에 설정해줄 수 도 있다. 

```
DATABASE_HOST=your-unique-url.rds.amazonaws.com
DATABASE_PORT=5432
DATABASE_NAME=strapi
DATABASE_USERNAME=postgres
DATABASE_PASSWORD=Password
AWS_ACCESS_KEY_ID=aws-access-key-id
AWS_ACCESS_SECRET=aws-access-secret
AWS_REGION=aws-region
AWS_BUCKET_NAME=my-project-bucket-name
```

아래 명령어로 pm2를 시작한다. 

```
cd ~
pm2 start ecosystem.config.js
```

이제 ```http://ubuntu서버주소:1337/``` 로 접근해 strapi를 사용할 수 있다. 


ps.... 

aws 배포 후 프론트에서 해당 서버에 접근 시 500 에러가 발생한다. 

permission 관련으로 접근이 안되는 것 같은 데 아직 해결 못 했다. 

또 해당 서버의 strapi 프로젝트에서 documents 사용 시 server가 0.0.0.0:1337로 접근해 cors 에러가 발생한다. 







---
layout: post
title:  "Next.js Study "
author: "JJoo"
comments: true
tags: next.js
---

## 스터디 목표

: state & props를 자유자재로 사용해보자.



## 작업 시안
![Design](https://raw.githubusercontent.com/JJooStudy/NextjsStudy/master/static/design.png "Design")



## 작업물 : [github.com/JJooStudy](https://github.com/JJooStudy/NextjsStudy)



## 1. Next.js 기본 구조 파악
- Next.js의 디렉토리 구조 파악
- 코드 스플리팅
: pages 폴더 안에 파일들이 page=라우터가 된다는 것. Components 폴더 안에 각 컴포넌트들이 들어간다는 것


## 2. 컴포넌트의 구조화(모듈화)

{% highlight text %}
components
├─Button
  ├─ButtonDiv.js
  ├─Buttons.js
  └─SaveDeleteDiv.js
├─Input
  ├─Input.js
  └─Inputs.js
{% endhighlight %}


component/ButtonDiv.js : jsx, props, state 등 데이터 처리 및 이벤트에 대한 내용을 담고 있음

{% highlight react %}
class ButtonDiv extends Component {
    render(){
        const { title, subTitle, data , handleSelect, selected, multiple } = this.props
        return(
            <>
                <Title>{title}<span>{subTitle}</span></Title>
                <ButtonWrap>
                    {
                        data.map((item, idx) => {
                            // console.log('selected:',selected)
                            // console.log('multiple:',multiple)
                            const active = multiple
                                ? selected.includes(item.id)
                                : selected === item.id
                            return (
                                <Button.Button onClick={() => handleSelect(item.id)} multiple={multiple} active={active} key={idx}>{item.name}</Button.Button>
                            )
                        })
                    }
                </ButtonWrap>
            </>
        )
    }
}
{% endhighlight %}


component/Buttons.js : Button에 관련된 styled-components를 담고 잇음 

{% highlight react %}
export const Button = styled.button`
    flex:1 1 calc(33.33% - 10px);
    margin:0 5px;
    margin-bottom:7px;
    padding:7px 0;
    box-sizing:border-box;
    border:1px solid #000;
    color:gray;
    font-size:14px;
    border-radius:5px;
    background-color:#fff;
    ${props => props.active && css`
        color:#fff;
        background-color: #000;
    `}
    // 삭제 임시저장 버튼
    ${props => props.gray && css`
        border-color:#f1f1f1;
        // font-size:15px;
        font-weight:bold;
        background-color:#f1f1f1;
        :hover, :focus, :active {
            color:#fff;
            background-color:#000;
        }
    `}

`
{% endhighlight %}



## 3. 같은 Button 컴포넌트 호출 후 개별 작동

: 재사용성을 높이기 위한 작업 훈련

- 한 화면에(ex. Step1.js) 동일한 자식 컴포넌트인 ButtonDiv.js를 다중 생성해 하나의 ButtonDiv에서만 다중 선택이 가능하도록 구현


container/Step1.js

{% highlight react %}
class Step1 extends Component {
    state = {
        selected: 0,
        selected2: [],
        selected3: 0,
    }
    
    handleSelect = (selected) => {
        this.setState({selected})
    }
    handleSelect2 = (selected2) => {
        const currentData = this.state.selected2
        if(currentData.includes(selected2)){
            const indexValue = currentData.indexOf(selected2)
            console.log(indexValue)
            currentData.splice(indexValue, 1)
        }else{
            currentData.push(selected2)
        }
        this.setState({selected2: currentData})
        console.log(currentData)
    }
    handleSelect3 = (selected3) => {
        this.setState({selected3})
    }

  render(){
        const { selected, selected2, selected3 } = this.state
        const { handleNextStep } = this.props
        return(
            <Container>
                <ButtonDiv handleSelect={this.handleSelect} multiple={false} selected={selected} title="동물종류 *" data={animal}  />
                <Inputs label="품종을 입력해 주세요" />
                <Inputs label="생후 개월 수를 입력해 주세요" />
                <ButtonDiv handleSelect={this.handleSelect2} multiple={true} selected={selected2} title="접종 및 기타사항" subTitle="복수선택 가능" data={vaccinationEtc} />
                <Inputs title="분양 소개 및 설명 *" label="분양에 대한 설명을 작성해 주세요" />
                <ButtonDiv handleSelect={this.handleSelect3} multiple={false} selected={selected3} title="분양글 게시기간" data={date} />
                <Inputs title="분양위치 지정 *" label="분양하실 위치를 지정하세요" />
                <Button.ButtonStep onClick={() => handleNextStep(1)}>
                    다음단계(아이정보입력)
                </Button.ButtonStep>
            </Container>
        )
    }
}
export default Step1;
{% endhighlight %}


- components/Button/ButtonDiv.js
: Step1.js에서 받아오는 props.multiple의 값에 따라 multiple값이 true이면 selected.includes(item.id), false면 selected === item.id가 적용되어 버튼의 다중선택이 될 수 있게 구현. 

{% highlight react %}
class ButtonDiv extends Component {
    render(){
        const { title, subTitle, data , handleSelect, selected, multiple } = this.props
        return(
            <>
                <Title>{title}<span>{subTitle}</span></Title>
                <ButtonWrap>
                    {
                        data.map((item, idx) => {
                            const active = multiple
                                ? selected.includes(item.id)
                                : selected === item.id
                            return (
                                <Button.Button onClick={() => handleSelect(item.id)} multiple={multiple} active={active} key={idx}>{item.name}</Button.Button>
                            )
                        })
                    }
                </ButtonWrap>
            </>
        )
    }
}
{% endhighlight %}



## 4. 페이지 변경없이 한 화면에서 step1, step2 화면 변경 가능

: state, props에 대한 이해를 위한 작업. 자식인 Step1.js에서 state를 받아 index.js에서 화면 변경


pages/index.js

{% highlight react %}
class Home extends React.Component {
  state = {
    step : null
  }
  componentDidMount(){
    this.setState({
      step : 0
    })
  }
  handleNextStep = (step) => {
    this.setState({
      step : step
    })
  }
  
  render() {
    return ( 
      <div>
        <h2>분양 글등록</h2>
        {/* <Step1 handleNextStep={this.handleNextStep} /> */}
        {
          this.state.step === 0 &&  ( <Step1 handleNextStep={this.handleNextStep} /> )
        }
        {
          this.state.step === 1 &&  ( <Step2 /> )
        }
      </div>
    )
  }
}
export default Home
{% endhighlight %}


containers/Step1.js

{% highlight react %}
class Step1 extends Component {
    render(){
        const { handleNextStep } = this.props
        return(
            <Container>
                <Button.ButtonStep onClick={() => handleNextStep(1)}>
                    다음단계(아이정보입력)
                </Button.ButtonStep>
            </Container>
        )
    }
}
export default Step1;
{% endhighlight %}



## 5. common/db.js에 데이터 분리

: static한 데이터 관리



## 6. 각 개체에 대한 정보(사진들, 성별정보, 설명, 금액 등)를 담는 컴포넌트를 원하는 만큼 추가

: 자식이 여러개가 존재하는 컴포넌트를 다중 생성했을 때 state를 다루는 방식에 대한 공부


containers/Step2.js
: ButtonAdd 버튼이 눌릴때 마다 PetItem 생성 

{% highlight react %}
class Step2 extends Component {
    state = {
        // selected: 0,
        petItem : [
            {id:0, gender: 0}
        ]
    }
    handleSelect = (selected) => {
        this.setState({selected})
        // console.log(selected)
    }
    addPetItem = () => {
        const copyPetItem = Object.assign([], this.state.petItem);
        console.log('copyPetItem', copyPetItem)
        if(copyPetItem.length === 0){
            copyPetItem.push({id:0})
            this.setState({petItem:copyPetItem})
        }else{
            copyPetItem.push({id:copyPetItem.length})
            this.setState({petItem:copyPetItem})
        }
    }
    
    render(){
        const { petItem } = this.state
        return(
            <>
                <Container>
                    {
                        petItem.map((pet) => {
                            return <PetItem handleSelect={this.handleSelect} selected={pet.gender} pet={pet} key={pet.id} />
                        })
                    }
                    {/* <PetItem handleSelect={this.handleSelect} selected={selected}/> */}
                    <Button.ButtonAdd onClick={() => this.addPetItem()}><span>아이 추가하기</span></Button.ButtonAdd>
                    <Button.ButtonEnd>분양글 등록하기</Button.ButtonEnd>
                </Container>
            </>
        )
    }
}

export default Step2;
{% endhighlight %}



## 7. 6에서 추가된 PetItem안의 버튼들이 개별 작동할 수 있도록 수정

: 현재 수정중


components/PetItem/PetItem.js
: Step2.js에서 불러오는 PetItem들 안의 PhotoBox, ButtonDiv가 각 컨포넌트별로 개별 작동 할 수 있도록 추가 수정 필요 

{% highlight react %}
class PetItem extends Component {
    state = {
        display:true
        
    }
    render(){
        const { handleSelect, selected, pet } = this.props
        console.log("selected : ",selected)
        return (
            <>
                <Title underLine onClick={() => this.setState({display: !this.state.display})}>아이 {pet.id+1} 정보 *</Title>
                <div>
                    <PhotoBox />
                    <ButtonDiv handleSelect={handleSelect} selected={selected} data={gender} />
                    <Inputs label="아이에 대한 설명을 작성해 주세요" />
                    <Inputs label="분양 금액을 입력해 주세요" />
                    <SaveDeleteDiv />
                </div>
            </>
        )
    }
}
export default PetItem;
{% endhighlight %}



## 8. 사진 추가 기능 
![AddImagesDesign](https://raw.githubusercontent.com/JJooStudy/NextjsStudy/master/static/addImages.png)


## 9. 마지막 분양글 등록하기 버튼 클릭시 step1과 step2에서 받은 정보를 한번에 저장.

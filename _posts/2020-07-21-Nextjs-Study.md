---
layout: post
title:  "Next.js Study "
author: "JJoo"
comments: true
---

스터디 목표

: state & props를 자유자재로 사용해보자.



## 1. Next.js 기본 구조 파악
- Next.js의 디렉토리 구조 파악
- 코드 스플리팅
: pages 폴더 안에 파일들이 page=라우터가 된다는 것. Components 폴더 안에 각 컴포넌트들이 들어간다는 것

## 2. 컴포넌트의 구조화

{% highlight ruby %}
components
├─Button
  ├─ButtonDiv.js
  ├─Buttons.js
  └─SaveDeleteDiv.js
├─Input
  ├─Input.js
  └─Inputs.js
{% endhighlight %}


- component/ButtonDiv.js : jsx, props, state 등 데이터 처리 및 이벤트에 대한 내용을 담고 있음

{% highlight ruby %}
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

- component/Buttons.js : Button에 관련된 styled-components를 담고 잇음 

{% highlight ruby %}
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


Head over to the [Example Content]({{ site.baseurl }}/2017-03-16/example-content) post for a showcase of Tale's text formatting features.

## Browser Support
Tale works on most if not all modern browsers, including Chrome, Safari and Firefox 👍🏼

## Download or Contribute
Tale is publicly hosted on GitHub, so go ahead and download or fork it at the [GitHub repository](https://github.com/chesterhow/tale). If you spot any bugs or have any suggestions, feel free to create an issue or make a pull request.

Thanks for checking out Tale!


{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

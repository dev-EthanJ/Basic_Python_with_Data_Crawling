# Using Markdown 마크다운으로 문서 작성하기 

## 0. 원문

[https://gist.github.com/ihoneymon/652be052a0727ad59601](https://gist.github.com/ihoneymon/652be052a0727ad59601)  
[https://www.markdownguide.org/getting-started/](https://www.markdownguide.org/getting-started/)

## 1. 마크다운에 관하여

### 1.1 마크다운이란?

> -   텍스트 기반의 마크업 언어
> -   웹에서 쉽고 빠르게 작성, 직관적 독해 가능  
>     → HTML로 변환 가능
> -   Github "README.md": repo 정보 기록 마크다운 문서  
>     → 설치 방법, 소스코드 설명, 이슈 기록 및 가독성 up!

### 1.2 마크다운의 장/단점

#### 1.2.1. 장점

> 1.  간결하다
> 2.  별도의 도구없이 작성가능하다
> 3.  다양한 형태로 변환 가능
> 4.  Text 저장 → 용량↓ → 보관 용이
> 5.  지원 프로그램 및 플랫폼 다양

#### 1.2.2. 단점

> 1.  표준이 없다
> 2.  도구에 띠라서 변환방식이나 생성물이 다르다
> 3.  모든 HTML 마크업을 대신할 수 없다

## 2. 마크다운 사용법(문법)

### 2.1. Headers

-   큰제목 : 문서 제목 > # "Title"
-   작은제목 : 문서 부제목 > ## "sub-title"
-   헤더 : # ~ ###### (1~6) 까지만 지원

#### 2.2. BlockQuote

-   이메일에서 사용하는 '>' 블럭인용문자를 이용

This is a first blockquote

> This is a second blockquote
> 
> ```
> > This is a third blockquote.
> ```

-   blockquote 안에서 다른 markdown item 포함 가능

### This is H3

-   -   List
        
        ```
        code
        ```
        

### 2.3. 목록 (List)

-   순서있는 목록(Ordered List) > '숫자' + 'dot' + ' '(space)

```
1. 첫 번째
2. 두 번째
3. 세 번째
```

1.  첫 번쨰
2.  두 번째
3.  세 번째

-   어떤 번호를 입력해도 순서는 내림차순으로 정의됨

```
1. first
3. third
2. second
```

1.  first
2.  third
3.  second

-   순서없는 목록(글머리 기호: `*`, `+`, `-` 지원)

```
* 빨강
    * 녹색
        * 파랑

+ 빨강
    + 녹색
        + 파랑

- 빨강
    - 녹색
        - 파랑

- R
  - G
    - B
```

-   빨강
    -   녹색
        -   파랑
-   빨강
    -   녹색
        -   파랑
-   빨강
    -   녹색
        -   파랑
-   R
    -   G
        -   B
-   혼합 사용 가능

```
* 1단계
  - 2단계
    + 3단계
      + 4단계
```

-   1단계
    -   2단계
        -   3단계
            -   4단계

### 2.4. 코드

-   4개의 공백 or tab == 들여쓰기 > 변환 시작 > 들여쓰지 않은 행까지 변환 유지

```
This is a normal paragraph:

    This is a code block.

end code block.
```

This is a normal paragraph:

```
This is a code block.
```

end code block.

-   한줄 enter X > 인식 X

```
This is a normal paragraph:
    This is a code block.
end code block.
```

This is a normal paragraph:  
This is a code block.  
end code block.

#### 2.4.1. 코드블럭

-   코드블럭 이용방식

1.  css 스타일

```
<pre><code>{code}</code></pre>
```

e.g.

```
<pre>
<code>
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello, Honeymon");
  }

}
</code>
</pre>
```

```

public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello, Honeymon");
  }

}
```

2.  code block 코드(\`\`\`로 열고 닫기) 이용
    
    ````
    ```
    code
    ```
    ````
    

e.g.

````
```
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello, Honeymon");
  }
}
```
````

```
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello, Honeymon");
  }
}
```

**Github**에서는 코드블럭 code시작점에 사용하는 언어를 선언하여 [문법강조(Syntax highlighting)](https://docs.github.com/en/github/writing-on-github/creating-and-highlighting-code-blocks#syntax-highlighting)이 가능하다.

````
```java
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello, Honeymon");
  }
}
```
````

```
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello, Honeymon");
  }
}
```

### 2.5. 수평선 `<hr>`

-   아래 줄은 모두 수평선을 만든다. 마크다운 문서를 미리보기로 출력할 때 _페이지 나누기_ 용도로 많이 사용한다.

```
* * *

***

*****

- - -

---------------------------------------
```

---

---

---

---

---

### 2.6. 링크

-   참조링크

```
[link keyword][id]

[id]: URL "Optional Title here"
```

e.g.

```
Link: [Google][googlelink]

[googlelink]: https://google.com "Go google"
```

@ Obsidian에서는 다음과 같이 표시되지만

Link: [Google](https://google.com "Go google")

@ 일반적인 markdown에선 다음과 같다

Link: [Google](https://google.com "Go google")

-   외부링크

```
사용문법: [Title](link)
적용예: [Google](https://google.com, "google link")
```

[Google](https://google.com, "google link")

-   자동연결

```
일반적인 URL 혹은 이메일주소인 경우 적절한 형식으로 링크를 형성한다.

* 외부링크: <http://example.com/>
* 이메일링크: <address@example.com>
```

-   외부링크: [http://example.com/](http://example.com/)
-   이메일링크: [address@example.com](mailto:address@example.com)

### 2.7. 강조

```
*single asterisks*
_single underscores_
**double asterisks**
__double underscores__
~~cancelline~~
```

_single asterisks_  
_single underscores_  
**double asterisks**  
**double underscores**

~cancelline~

```
문장 중간에 사용할 경우에는 **띄어쓰기** 를 사용하는 것이 좋다.
```

문장 중간에 사용할 경우에는 **띄어쓰기** 를 사용하는 것이 좋다.

### 2.8. image

-   image 첨부

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
```

e.g.

```
![재찬이1](D:\picture\재찬_분홍니트.jpg)
![재찬이2](D:\picture\재찬_분홍니트.jpg "사랑해")
```

![재찬이1](D:%5Cpicture%5C%EC%9E%AC%EC%B0%AC_%EB%B6%84%ED%99%8D%EB%8B%88%ED%8A%B8.jpg)

![재찬이2](D:%5Cpicture%5C%EC%9E%AC%EC%B0%AC_%EB%B6%84%ED%99%8D%EB%8B%88%ED%8A%B8.jpg "사랑해")

-   사이즈 조절 기능은 없기 때문에  
    `<img width="" height=""></img>`  
    를 이용한다.

```
<img src="/path/to/img.jpg" width="Npx" height="Npx" title="TITLE" alt="ALT_TITLE"></img><br/>
<img src="/path/to/img.jpg" width="N%" height="N%" title="TITLE" alt="ALT_TITLE"></img>
```

e.g.

```
<img src="D:\picture\재찬_분홍니트.jpg" width="100px" height="150px" title="재찬" alt="분홍니트"></img><br/>
```

![분홍니트](D:\picture\재찬_분홍니트.jpg "재찬")

```
<img src="D:\picture\재찬_분홍니트.jpg" width="20%" height="20%" title="재찬" alt="분홍니트"></img><br/>
```

![분홍니트](D:\picture\재찬_분홍니트.jpg "재찬")

### 2.9. 줄바꿈

-   문장 마지막에 3칸 이상 띄어쓰기() + enter 입력을 하면 줄이 바뀐다.
-   `<br>` 키워드로도 가능

```
* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다. 그냥 enter 입력하면,
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다.___\\띄어쓰기   
이렇게   
```

@ Obsidian 환경에서는 아래와 같다

-   줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다. 그냥 enter 입력하면,  
    이렇게
-   줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다.\_\_\_\\띄어쓰기  
    이렇게

@ 일반적인 Markdown환경에서는 아래와 같다

-   줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다. 그냥 enter 입력하면,  
    이렇게
-   줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기해야 한다.\_\_\_\\띄어쓰기  
    이렇게

```
* 띄어쓰기 하고 엔터를 안치면?   이렇게 끝.
* 띄어쓰기 안하고 엔터를 치면?
이렇게 끝.
* `<br>` 키워드를 사용하면?<br>이렇게 끝.
```

-   띄어쓰기 하고 엔터를 안치면? 이렇게 끝.
-   띄어쓰기 안하고 엔터를 치면?  
    이렇게 끝.
-   `<br>` 키워드를 사용하면?  
    이렇게 끝.

## 3. 마크다운 사용기

### 3.1. 위지윅(WSYWIG) 에디터

-   우리가 흔하게 웹에서 사용되는 에디터(Naver, Daum, Google, etc.) >  
    기본적으로 HTML이용해 스타일 적용 > 하루패드 등 마크다운 에디터 View 영역 복붙 > 대체로 View영역 보이는 그대로 복사
-   if 붙여넣기 이후 문장들 수정 > 스타일 태그 수정시 변형 > 전체적 영향 있음 > 복붙시 가급적 본문 수정은 안하는 게 좋다

### 3.2. Github, Bitbucket, Yobi, etc.

-   최근 유행 협업 개발 플랫폼 > 대부분 마크다운 컨버터 기본 탑재 >  
    마크다운 텍스트 그래도 복붙 or 업로드 > 마크다운 적용됨

### 3.3. MS워드

-   View영역 항목 그대로 복붙 or HTML 내보내기 생성 파일 불러오기
-   적용된 헤더를 워드가 목차에 적용> 추가 목차 수정 필요X

## 4. 정리

-   마크다운 == 기본 문법만 알고있다면 일반 텍스트편집기에서도 손쉽게 작성 가능한 마크업 > 현재 다양한 도구와 플랫폼에서 지원중

마크다운을 이해하고 사용하면서 쉽고 빠르게 스타일문서를 작성해보세요.

-   Dropbox pro > home-laptop-phone 각각 연동 가능 > 저장된 Markdown 문서 >  
    Dropbox 웹서비스 상에서 제공 > 웹에서 바로 열람 가능 > 링크를 걸어서 다른 사람과 공유 가능

## P.S.

-   Notion에서 작성한 문서 > [Atom](http://atom.io/), [Visual Studio Code](https://code.visualstudio.com/), [Notepad++](https://notepad-plus-plus.org/) 텍스트 편집기에 복붙 > markdown으로 작성된 문장 생성 > 이지윅 에디터 제공 웹에디터 붙여넣기 > 거의 완벽한 형태로 복사됨

## References

-   [78 Tools for writing and previewing Markdown](http://mashable.com/2013/06/24/markdown-tools/)
-   [John gruber 마크다운 번역](http://nolboo.github.io/blog/2013/09/07/john-gruber-markdown/)
-   [깃허브 취향의 마크다운 번역](http://nolboo.github.io/blog/2014/03/25/github-flavored-markdown/)
-   [허니몬의 마크다운 작성법](http://www.slideshare.net/ihoneymon/ss-40575068)
-   Notion.so([https://www.notion.so/product](https://www.notion.so/product))
-   Atom([https://atom.io/](https://atom.io/))
-   Visual Studio Code([https://code.visualstudio.com/](https://code.visualstudio.com/))
-   Notepad++([https://notepad-plus-plus.org/](https://notepad-plus-plus.org/))
# π Introduction

> **CSR? SSR?** μ€λ λ€μ νλ² κ·Έκ² λ­μ§ ννμ³λ³΄μ.

"Reactλ CSR κΈ°λ°μ SPAλ₯Ό κ΅¬μΆνκΈ° μν νλ μμν¬λ€." λΌλ λ§μ μ λ§ λ§μ΄λ λ€μμ§λ§, μ¬νκΉμ§ νμλ μ΄κ²λ€μ΄ λ­ μ°¨μ΄μ μ΄ μλμ§λ₯Ό μ μμ§ λͺ»νλ€. CSRμ΄λ SPAλ κ²°κ΅­ ν νμ΄μ§μμ λ λλ§ λλ κ±°λκΉ κ°μ μλ―Έκ° μλκΉλΌκ³ λ§ μκ°νμ§, μ ννκ² μ΄κ²λ€μ΄ λ¬΄μ¨ μ°¨μ΄κ° μλμ§λ₯Ό κ·Όλ³Έμ μΌλ‘ μμ§ λͺ»νλ€.

μ΅κ·Ό μ°μ°ν κΈ°νμ νμκ° React μ€ν°λλ₯Ό μ£Όλνκ² λμλλ°, λ§μ κ»μ§μ κΉκ³  λ³΄λ λλ μ λ§ Reactλ₯Ό μ μ°λμ§ μκ³  μλ μΆμ μκ°μ΄ λ€μλ€. λ°λΌμ μ΄λ₯Ό λͺνν μκΈ° μν΄ CSR, SSR, SPA, MPA κ°λμ μ§κ³  λμ΄κ°κΈ°λ‘ κ²°μ¬νκ³ , μ κ°λλ€μ λν λμ μ‘°μ¬ κ²°κ³Όλ₯Ό μ λ¦¬νκΈ°λ‘ νλ€.

# βοΈ Static Web Page

### 1. Static Site λ?

-   μλ²μμ μ¬μ μ λ λλ§ λ μ μ  λ¦¬μμ€λ‘ λ§λ€μ΄μ§ μ¬μ΄νΈ.
-   κ³Όκ±°μλ μλ²μμ μ λ§λ€μ΄μ§ HTMLλ₯Ό ν΄λΌμ΄μΈνΈμκ² μ μ‘νμμ.
-   μ΄μ°½κΈ° μΉ μ¬μ΄νΈλ λ¨μν μ λ³΄λ§μ μ κ³΅νκ³ , λ§μ Interactionμ μκ΅¬νμ§ μμκΈ° λλ¬Έ.
-   μ΄λλ κ±°μ HTMLκ³Ό CSSλ‘λ§ λͺ¨λ  νμ΄μ§λ₯Ό κ΅¬μΆνμμ. JSλ λμ€μ λμ΄.

#### 1-1. Static Web Page μ μ₯λ¨μ μ?

1. μ₯μ 

-   μ²« μμ²­μ λν νμΌλ§ μ μ‘νλ©΄ λκΈ°μ λΉ λ₯΄λ€.
-   λ¨μν HTML λ¬Έμλ§μΌλ‘ μΉμ κ΅¬μΆνκΈ°μ μλ²μ νΈλν½μ΄ μλμ μΌλ‘ μ λ€.

2. λ¨μ 

-   μ μ₯λ μ μ  νμ΄μ§λ§μ λ³΄μ¬μ£ΌκΈ°μ λ¨μν μλΉμ€λ§ μ κ³΅μ΄ κ°λ₯νλ€.
-   νμ΄μ§ λ΄μμ λ€λ₯Έ λ§ν¬λ₯Ό ν΄λ¦­ν  κ²½μ°, HTMLμ λ€μ λ°μ λ‘λ©νλ―λ‘ "κΉλΉ‘μ" νμμ΄ λ°μνλ€.
-   μμ λ³νμμλ νμ΄μ§ νλλ₯Ό ν΅μ§Έλ‘ λ€μ λ‘λ©ν ν μ κ³΅ν΄μΌ νλ―λ‘, μ¬μ¬μ©μ±μ΄ λ¨μ΄μ§λ€.

> **HTML (Hyper Text Markup Language)** λ?

1. μ©μ΄μ μ μ

-   `Hyper Text` λ νμ΄νΌ λ§ν¬λ₯Ό μ¬μ©νμ¬ μ¬μ©μκ° μνλ μμμ λ°λΌ λ€λ₯Έ λ¬Έμλ‘ μ κ·Όνλ νμ€νΈλ₯Ό μλ―Έ.
-   `Markup Language` λ νκ·Έλ₯Ό μ΄μ©νμ¬ λ¬Έμλ λ°μ΄ν°μ κ΅¬μ‘°λ₯Ό λͺμνλ μΈμ΄λ₯Ό μλ―Έ.
-   μ΄λ₯Ό μ’ν©νλ©΄ `HTML` μ **μΉ νμ΄μ§μ κ΅¬μ‘° νΉμ λ°μ΄ν° μμ±μ μν΄** μ°μ΄λ λ§ν¬μ μΈμ΄μ΄λ€.

2. μ°μ΄λ μ΄μ λ?

-   μΉ λΈλΌμ°μ μ λ΄κ° μ μν μ»¨νμΈ λ₯Ό μ ννκ² λ³΄μ¬μ£ΌκΈ° μν΄ λ§ν¬μ μΈμ΄κ° μ°μ.
-   λ€λ₯Έ μ¬μ©μλ€κ³Όμ μνν νμμ μν΄ κ³΅ν΅λ μμμ μ¬μ©ν  νμμ±μ΄ μ»€μ‘μ.
-   μ λ³΄λ₯Ό μμ§νλ κ²μ μμ§μμ μλ£λ₯Ό μμ§ν  μ μλλ‘ νκ·Έλ₯Ό ν΅ν΄ κ°κ°μ μ λ³΄λ€μ λͺμν΄μΌ νκΈ° λλ¬Έ.

# βοΈ Dynamic Web Page

### 1. Dynamic Web Page λ?

-   κΈ°μ μ΄ λ°μ νλ©΄μ λ³΄λ€ ν­λμ μλΉμ€λ₯Ό μ κ³΅νκΈ° μν΄μλ λμ μΈ μ²λ¦¬κ° νμν΄μ‘λ€.
-   μ΄κΈ° λ λλ§ μ΄ν μλ²λ‘λΆν° νμν λ°μ΄ν°λ₯Ό μΆκ°μ μΌλ‘ μμ²­νμ¬, νμ΄μ§ μΌλΆλ₯Ό λ³κ²½νλ κΈ°μ μ΄ λ±μ₯.
-   μΉ νμ΄μ§μμ νμν μ λ³΄κ° μμ κ²½μ°, μ΄λ₯Ό λμ μΌλ‘ μλ²μμλΆν° λ°μ **μΌλΆλ₯Ό λ³κ²½μν€λ** νμ΄μ§λ€.

### 2. AJAX (Asynchronous JavaScript and XML)

-   `AJAX (Asynchronous JavaScript and XML)` κ° λ±μ₯νλ©΄μ, μλ²μ λΈλΌμ°μ κ° λΉλκΈ°μ μΌλ‘ ν΅μ νκΈ° μ©μ΄ν΄μ§.
-   μ΄λ λΈλΌμ°μ μ μλ² κ°μλ `JSON`, `XML`, `HTML` λ±κ³Ό κ°μ λ°μ΄ν°λ₯Ό λΉλκΈ°λ‘ μ£Όκ³  λ°μ μ μμ.
-   μ΄μ λ μλ‘κ³ μΉ¨μ νμ§ μκ³  νλμ νμ΄μ§μμ λμ μΌλ‘ νμν λΆλΆλ§ λ³κ²½μ΄ κ°λ₯ν΄μ§κ² λ¨. (SPA)

1.  ` XMLHttpRequest` κ°μ²΄λ₯Ό μ¬μ©νμ¬ Ajax ν΅μ μ κ΅¬νν κ²½μ°.
    ` XMLHttpRequest` λ κ°μ₯ μ΄κΈ°μ μλ²λ‘λΆν° λ°μ΄ν°λ₯Ό λμ μΌλ‘ λ°μΈλ©νκΈ° μν΄ μ°μ΄λ κ°μ²΄.

```javascript
var xhr = new XMLHTtpRequest();
xhr.open('GET', './time.php');
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
        document.querySelector('#time').innerHTML = xhr.responseText;
    }
};
xhr.send();
```

2.  jQueryλ₯Ό νμ©νμ¬ Ajax ν΅μ μ κ΅¬νν κ²½μ°.

```javascript
var serverAddress = 'https://hacker-news.firebaseio.com/v0/topstories.json';

$.ajax({
    url: ,
    type: 'GET',
    success: function onData (data) {
        console.log(data);
    },
    error: function onError (error) {
        console.error(error);
    }
});
```

# βοΈ Client, Server Side Rendering

### 1. Client Side Rendering (CSR)

-   κΈ°μ μ΄ λ°μ νλ©΄μ μΉ νμ΄μ§μμ μ κ³΅νλ μλΉμ€κ° λ³΅μ‘ν΄μ§μΌλ‘μ, AJAXλ§μΌλ‘λ λͺ¨λ  λ‘μ§μ μ²λ¦¬νκΈ°κ° μ΄λ €μμ§.
-   μ΄λ₯Ό μν΄ **React, Vue, Angular** μ κ°μ νλ μμν¬κ° λ±μ₯νλ©΄μ, ν΄λΌμ΄μΈνΈμμ λ λλ§μ μ§ννλ κ²½μ°κ° λ§μμ§.
-   μ΄μ λ μλ²μμ μ΅μνμ HTMLμ λ°κ³ , λ΄λΆμ μμΉν script νκ·Έλ‘ JS νμΌμ λ°μ ν΄λΌμ΄μΈνΈμμ νμ΄μ§λ₯Ό λ λλ§νλ νμμΌλ‘ λ³ν.
-   μ΄κΈ°μ μ νλ¦¬μΌμ΄μ κ°λμ νμν JS νμΌμ ν¬κΈ°κ° ν¬κΈ°μ, μ κ·Ήμ μΈ μ½λ λΆν  (Code Splitting) μ κ³ λ €ν΄μΌ ν¨.

![](https://velog.velcdn.com/images/rookieand/post/fcde1395-cbed-4899-a726-d1ef59ad467e/image.webp)

#### 1-1. CSRμ λμ λ°©μ

1. μ¬μ©μκ° μΉ νμ΄μ§λ₯Ό λ°©λ¬Έν  κ²½μ°, λΈλΌμ°μ λ μ΅μνμ HTML νμΌμ λ€μ΄λ‘λνλ€.
2. λΈλΌμ°μ λ script νκ·Έλ₯Ό ν΅ν΄ JS λ²λ€ νμΌμ λ€μ΄λ‘λ νκ³ , Ajaxλ₯Ό ν΅ν΄ λμ μΌλ‘ μ»¨νμΈ λ₯Ό κ°μ Έμ€λ©° νλ©΄μ λ λλ§νλ€.
3. μ΄ν μ¬μ©μκ° νμ΄μ§λ₯Ό μ΄λν  κ²½μ°, λ³λμ HTMLμ λ°μ§ μκ³  μ¬μ μ λ°μ JS νμΌλ§μ νμ©νμ¬ λ λλ§μ μ§ννλ€.

![](https://velog.velcdn.com/images/rookieand/post/fce6f6b1-c0ff-46be-9284-8884035ddaf8/image.png)

#### 1-2. CSRμ μ₯λ¨μ 

1. μ₯μ 

-   μ΄κΈ° λ λλ§ μ΄νμ μ§νλλ λ λλ§μ κ²½μ° λ‘λ© μλκ° ν¨μ¬ λΉ λ₯΄λ€. μ΄λ―Έ νμ΄μ§ λ‘λ©μ νμν λͺ¨λ  μμμ λ°μ μνκΈ° λλ¬Έμ΄λ€.
-   μλ²μμ ν΅μ  κ³Όμ μμ νμ΄μ§μ UIλ₯Ό λ€μ κ·Έλ¦΄ νμ μμ΄, λμ μΌλ‘ μΌλΆλΆλ§ λ³κ²½μν¬ μ μλ€.

2. λ¨μ 

-   μ΄κΈ°μ κ΅¬λμ νμν νμΌμ μ λΆ λ°μμΌ νκΈ°μ, κ·Έλ§νΌμ μ΄κΈ° νμ΄μ§ κ΅¬λ μλκ° λλ¦¬λ€. (FP, FCP λλ¦Ό)
-   μ΄ κ²½μ° νμ΄μ§κ° μ¨μ ν λ‘λ©λκΈ° μ κΉμ§λ νμ νλ©΄λ§μ λ΄μΌ νκΈ°μ, μ¬μ©μ κ²½νμ΄ μ’μ§ λͺ»ν¨.
-   SEO (Search Engine Optimization), μ¦ κ²μ μμ§ μ΅μ νλ₯Ό μ§ννκΈ° μ΄λ €μ. κ²μ μμ§μ΄ μ²μ νμ΄μ§λ₯Ό λ°©λ¬Έν  κ²½μ° HTMLμ΄ λΉ κ²½μ°κ° λ§κΈ° λλ¬Έ.
-   νμ΄μ§ λ©ν λ°μ΄ν°μ λ³κ²½μ μν μΆκ° μμμ΄ νμν¨. λ€λ₯Έ νμ΄μ§λ‘μ μ΄μ μ μ§νν  κ²½μ° μ΄λ₯Ό μΈμ§μν€κΈ° μν΄ κ° νμ΄μ§μ λν λ©ν λ°μ΄ν°λ₯Ό λμ μΌλ‘ λ λλ§ ν΄μΌ νκΈ° λλ¬Έ.
-   ν΄λΌμ΄μΈνΈμ PC μ±λ₯μ λ°λΌ λ λλ§ μλκ° μ’μ°λκΈ° λλ¬Έμ, PC μ±λ₯μ΄ μ’μ§ λͺ»ν  κ²½μ° λ λλ§μ λΉ λ₯΄κ² νμ§ λͺ»ν  νλ₯ μ΄ λμ.

### 2. Server Side Rendering (SSR)

-   μλ²μμ μ¬μ μ λ λλ§λ νμΌμ ν΄λΌμ΄μΈνΈμκ² μ μ‘νκ³ , ν΄λΌμ΄μΈνΈλ μ΄λ₯Ό μ¦μ λ λλ§νλ λ°©μμ΄λ€.
-   λ¨, JSμ κ²½μ° μ μ  λ¦¬μμ€ (HTML, CSS) κ° λ λλ§ λ μ΄ν μ μ©λκΈ°μ JSκ° λͺ¨λ μ μ©λκΈ° μ μλ μ‘°μμ΄ λΆκ°λ₯νλ€.

![](https://velog.velcdn.com/images/rookieand/post/9224530e-456a-442a-9c47-2873f09f46cc/image.webp)


#### 2-1. SSRμ λμ λ°©μ

1. μ¬μ©μκ° μΉ νμ΄μ§λ₯Ό λ°©λ¬Έν  κ²½μ°, μλ²λ μ΄λ₯Ό νμΈνκ³  λΈλΌμ°μ μκ² μ κ³΅ν  HTML μ»¨νμΈ λ₯Ό μ»΄νμΌνλ€.
2. μ»΄νμΌλ HTMLμ λΈλΌμ°μ μ μ κ³΅λλ©°, λΈλΌμ°μ λ μ΄λ₯Ό λ€μ΄λ‘λνκ³  λ λλ§νμ¬ μ¬μ©μκ° μ΄λ₯Ό λ³Ό μ μλλ‘ νλ€.
3. μ΄ν JS νμΌμ λ€μ΄λ‘λ λ°μ ν μ€ννμ¬ μ¬μ©μμ νμ΄μ§ κ°μ μΈν°λ μμ κ°λ₯νκ²λ νλ€.
4. μ¬μ©μκ° λ€λ₯Έ νμ΄μ§λ‘ μ΄λμ μ§νν  κ²½μ°, 1~3λ²μ κ³Όμ μ λ°λ³΅νλ€.

![](https://velog.velcdn.com/images/rookieand/post/4df6ccd8-332e-4ab6-a855-027c7c72efe1/image.png)


#### 2-2. SSRμ μ₯λ¨μ 

1. μ₯μ 

-   μ΄κΈ° νμ΄μ§ λ‘λ© μλκ° λΉ λ₯΄λ€. (FP, FCPκ° λΉ λ₯΄λ€) μλ²μμ μ¬μ μ λ λλ§ λ HTML νμΌμ λΈλΌμ°μ μμ λ‘λ©νκΈ° λλ¬Έμ΄λ€.
-   μλ²μμ νμ΄μ§ λ‘μ§ λ° λ λλ§μ μ¬μ μ μ€νν  κ²½μ°, JS νμΌμ λ§μ΄ λ³΄λΌ νμκ° μμ΄μ§λ―λ‘ TTI λν μλμ μΌλ‘ λΉ λ₯΄κ² μνν  μ μλ€.
-   μ΄λ―Έ μ¬μ μ λ λλ§ λ HTMLμ ν¬λ‘€λ§ λ΄μ΄ λ°©λ¬Ένλ―λ‘, SEO λν ν¨μ¨μ μΌλ‘ μ μ©ν  μ μλ€.
-   μλ²μμ μμ±λ νμ΄μ§λ₯Ό λΈλΌμ°μ μμ λ³΄μ¬μ£ΌκΈ°λ§ νλ©΄ λλ―λ‘, ν΄λΌμ΄μΈνΈμ PC μ±λ₯μ ν¬κ² μν₯μ λ°μ§ μλλ€.

2. λ¨μ 

-   νμ΄μ§λ₯Ό μ΄λν  λλ§λ€ λ§€λ² μλ‘μ΄ HTMLμ λ°μμΌ νλ―λ‘ `TTFB` (Time To First Byte) κ° λλ¦¬λ€.
-   μλ²μμ κ° νμ΄μ§μ λν μ μ  λ¦¬μμ€λ₯Ό λ³΄λ΄μΌ νκΈ° λλ¬Έμ μ΄λ₯Ό μν μλ² νΈμ€νμ΄ νμμ μ΄λ€.
-   κ° νμ΄μ§λ₯Ό λ‘λνλ κ³Όμ μ΄ μ€λ κ±Έλ¦°λ€λ©΄, λλ € μ¬μ©μ κ²½νμ ν΄μΉ  μ μλ€.
-   κ° μμ²­μ λν HTML νμΌμ κ·Έλ κ·Έλ λ λλ§ ν΄μΌ νκΈ° λλ¬Έμ, CDNμ νμ©ν μΊμ± μ λ΅μ μ¬μ©ν  μ μλ€.

> **TTFB(Time to First Byte)** λ λ­κΉ?

-   TTFBλ HTTP μμ²­μ λ³΄λΌ κ²½μ°, μλ²μμλΆν° μ²«λ²μ§Έ Byte (μ λ³΄) κ° μ€κΈ°κΉμ§ κ±Έλ¦¬λ μκ°μ μλ―Έν©λλ€.
-   CSRμ κ²½μ° μ²μ λ λλ§ μ΄νλΆν°λ μμ²­μ΄ νμν κ²½μ°μλ§ μλ²μμ νμν λ°μ΄ν°λ§μ λ°μμ€κΈ°μ TTFBκ° λΉ λ¦λλ€.
-   νμ§λ§ SSRμ κ²½μ° λ§€ νμ΄μ§ μ΄λ μλ§λ€ νμ΄μ§ λ λλ§μ νμν λ¦¬μμ€λ₯Ό λ°μμ€κΈ°μ μλμ μΌλ‘ TTFBκ° λλ¦½λλ€.

> **FP? FCP?** μ΄κ²λ€μ λ λ­κ°?

-   FP (First Paint) : μ²«λ²μ§Έ **ν½μ** μ΄ μ€ν¬λ¦°μ Paint λμμ λμ μκ°μ μλ―Ένλ€.
-   FCP (First Contentful Paint) : DOMμ μν μ»¨νμΈ μ μΌλΆκ° μ€ν¬λ¦°μ Paint λμμ λμ μκ°μ μλ―Ένλ€.
-   FP, FCPμ κ²½μ° Chromeμ timing APIλ‘ μΆμ μ΄ κ°λ₯νλ©°, GA κ°μ λκ΅¬λ‘ λ¦¬ν¬νμ΄ κ°λ₯νλ€.
-   μ¦ FPμ FCPμ μ¬μ© λͺ©μ μ μΌλ§λ λΉ¨λ¦¬ νλ©΄μ΄ λ λλ§ λλμ§λ₯Ό μΈ‘μ νκΈ° μν¨μ΄λ€.

> **CDN (Content Delivery Network)** μ΄λ?

μλ²μ μ¬μ©μ μ¬μ΄μ λ¬Όλ¦¬μ μΈ κ±°λ¦¬λ₯Ό μ’ν μ»¨νμΈ  λ‘λ© μλλ₯Ό μ΅μν νκΈ° μν μ μ‘ κΈ°λ². λ³΄ν΅ μ¬λ¬ κ³³μ μΊμ μλ²λ₯Ό λ ν μ΄ μ€μμ μ¬μ©μμ κ°μ₯ κ°κΉμ΄ μλ²λ₯Ό μ°Ύμ μμ²­ν μ»¨νμΈ λ₯Ό μ κ³΅νλ€.

# βοΈ Static Site Generator, Universal Rendering

### 1. SSG (Static Site Generator)

-   SSR μ²λΌ μλ²λ‘λΆν° λ λλ§λ HTMLμ λ°μμ€μ§λ§, HTMLμ μμ± μμ μ΄ **λΉλ νμ**μ΄λΌλ κ²μ΄ SSRκ³Όμ μ°¨μ΄μ μ΄λ€.
-   SSRμ κ²½μ° μμ²­μ΄ λ€μ΄μ€λ©΄ κ·Έλλ§λ€ νμν HTMLμ λ λλ§ν΄μΌ νμ§λ§, SSGμ κ²½μ° HTMLμ μ¬μ μ λΉλ νμμμ μμ±νμμΌλ―λ‘ μ΄λ₯Ό μ μ‘λ§ νλ©΄ λλ€.
-   μ μ  νμ΄μ§λ₯Ό μ κ³΅νλ κ²μ μλλ€. λΉλ νμμ μμ±λ HTML νμΌμ΄ μ μ μ΄λΌλ μλ―Έλ€. FCP μ΄ν JS νμΌμ λ°μ μΈν°λ μμ μ κ³΅ν  μλ μλ€.
-   Reactλ‘ κ°λ°λ μ νλ¦¬μΌμ΄μμ **Gatsby** λΌμ΄λΈλ¬λ¦¬λ₯Ό ν΅ν΄ λΉλνμ¬ μ μ μΈ μ¬μ΄νΈλ€λ‘ λ³νμμΌμ€λ€.

![](https://velog.velcdn.com/images/rookieand/post/d20183dd-1af0-4295-a5d4-ca5b8b4b61a5/image.webp)

#### 1-1. SSGμ λμ κ³Όμ 

1. μ¬μ©μκ° μΉ νμ΄μ§λ₯Ό λ°©λ¬Έν  κ²½μ°, μ£μ§ μΊμ±λ HTML νμΌμ ν΄λΌμ΄μΈνΈλ‘ λ°νν΄μ€λ€.
2. λΈλΌμ°μ λ HTMLμ λ°κ³  μ΄λ₯Ό λμ μ¬μ©μκ° μ¬μ΄νΈλ₯Ό λ³Ό μ μλλ‘ νλ€.

> μ£μ§ μΊμ± (Edge Caching) μ΄λ?

ν΄λΌμ΄μΈνΈμ μ»΄ν¨ν°μ μ΅λν κ°κΉκ² (λ¬Όλ¦¬μ μΈ κ±°λ¦¬) μ½νμΈ λ₯Ό μ μ₯νμ¬ λκΈ° μκ°μ μ€μ΄κΈ° μν΄ μΊμ± μλ²λ₯Ό μ¬μ©νλ κ². μ£Όλ‘ CDN μ λ΅μ λ§μ΄ μ¬μ©ν¨.

#### 1-2. SSGμ μ₯λ¨μ .

1. μ₯μ 

-   λΉλ νμμ κ° νμ΄μ§ λ³λ‘ HTML νμΌμ΄ μμ±λκΈ° λλ¬Έμ μλ²μμ μ¬μ  λ λλ§μ νμ§ μμλ λλ―λ‘ λΉ λ₯Έ FP, FCP λ₯Ό μ κ³΅.
-   μ΄λ―Έ μ¬μ μ λ λλ§ λ HTMLμ ν¬λ‘€λ§ λ΄μ΄ λ°©λ¬Ένλ―λ‘, SEO λν ν¨μ¨μ μΌλ‘ μ μ©ν  μ μλ€.

2. λ¨μ 

-   λͺ¨λ  νμ΄μ§ (URL) μ λν HTMLμ μμ±ν΄μΌ νλ€. λ°λΌμ λμ μΌλ‘ λ³νλ URL λ€μ λν λμμ΄ μ΄λ ΅λ€.

### 2. Universal Rendering

-   SSRμ ν΅ν΄ μ΄κΈ°μ λΉ λ₯Έ FCPλ₯Ό κ΅¬νν μ΄ν, ν΄λΌμ΄μΈνΈ λ¨μμμ `hydration` κΈ°λ²μ ν΅ν΄ JS νμΌμ μ μ©νμ¬ μΈν°λ μμ κ°λ₯νκ²λ νλ λ°©μ.
-   μ΄κΈ° λ‘λ© μμλ SSRμ²λΌ μλνκ³ , μ΄νμλ CSRλ‘ μλνλ λ°©μμ΄λ©° **Next.js, Nuxt.js** μ κ°μ νλ μμν¬λ₯Ό μ¬μ©νλ€.

![](https://velog.velcdn.com/images/rookieand/post/a5ab25db-1a48-4517-be56-3d0c2b3ac5aa/image.png)

-   CSR κΈ°λ²μ μ¬μ©νλ Reactμ κ²½μ°, μ΄κΈ° λ‘λ© μμλ ν λΉ νλ©΄μ λ³΄μ¬μ£Όκ³  λ λλ§ κ³Όμ μμ HTMLμ κ΅¬μΆνκ³  λ²λ€λ§λ JS μ½λλ₯Ό μ μ©νλ€.
-   Universal Renderingμ κ²½μ° μλ²μμ μ¬μ μ νμν HTMLμ λ λλ§νκ³ , μ΄ν ν΄λΌμ΄μΈνΈμμ λ²λ€λ§λ JS μ½λλ₯Ό μ μ©νλ€.

> Hydration μ΄λ?

JS μ½λ λ΄μ "μ΄λ²€νΈ νΈλ€λ¬" ν¨μλ€μ μ μ μΈ DOM λΈλλ€μ λΆμ¬μ λμ μΌλ‘ μνΈμμ©μ΄ κ°λ₯νλλ‘ λ°κΎΈμ΄μ£Όλ κΈ°λ₯μ΄λ€.
μ¦, μ¬μ©μκ° λ¨μν μΉ μ¬μ΄νΈμ νλ©΄μ λ³΄λ κ²μ΄ μλλΌ μ€μ§μ μΈ μνΈμμ©μ κ°λ₯νκ²λ νλ κ³Όμ μ΄λΌκ³  μκ°νλ©΄ λλ€.

#### 2-1. Universal Renderingμ λμ κ³Όμ 

1. μ¬μ©μκ° μΉ νμ΄μ§λ₯Ό λ°©λ¬Έν  κ²½μ°, μλ²λ μ΄λ₯Ό νμΈνκ³  λΈλΌμ°μ μκ² μ κ³΅ν  HTML μ»¨νμΈ λ₯Ό μ»΄νμΌνλ€.
2. μ»΄νμΌλ HTMLμ λΈλΌμ°μ μ μ κ³΅λλ©°, λΈλΌμ°μ λ μ΄λ₯Ό λ€μ΄λ‘λνκ³  λ λλ§νμ¬ μ¬μ©μκ° μ΄λ₯Ό λ³Ό μ μλλ‘ νλ€.
3. μ΄ν JS νμΌμ λ€μ΄λ‘λ λ°μ ν μ€ννμ¬ μ¬μ©μμ νμ΄μ§ κ°μ μΈν°λ μμ κ°λ₯νκ²λ νλ€.
4. μ¬μ©μκ° λ€λ₯Έ νμ΄μ§λ‘ μ΄λμ μ§νν  κ²½μ°, λ€μ΄λ‘λ λ°μ JS νμΌμ νμ©νμ¬ λ λλ§ (CSR) μ μ§ννλ€.

![](https://velog.velcdn.com/images/rookieand/post/5436b9ad-8a4d-4773-8c9f-c77370fa1fe8/image.webp)

#### 2-2. Universal Renderingμ μ₯λ¨μ .

1. μ₯μ 

-   SSR κΈ°λ²μ ν΅ν΄ λΉ λ₯Έ FCPλ₯Ό μ§μνλ―λ‘ TTVκ° λΉ λ₯΄λ€. μ΄λ κΈ°μ‘΄μ CSRμ΄ κ°μ§ λ¨μ μ μμν  μ μλ€.
-   μ΄κΈ° λ λλ§ μ΄νμλ μ¬μ μ λ°μ JS λ²λ€ νμΌμ νμ©νμ¬ λ λλ§μ΄ μ§νλλ―λ‘ SSRμ λ¨μ λ μμν  μ μλ€.

2. λ¨μ 

-   μλ²μμ μ΄κΈ° λ λλ§μ μννλ―λ‘ CSRμ κΈ°λ°μΌλ‘ κ°λ°μ μ§νν  κ²½μ° μ¬λ¬ λ¬Έμ μ μ λΆλͺν μ μλ€.
-   μ΄λ‘ μΈν λ¬λ μ»€λΈκ° μ‘΄μ¬νλ€. νμμ κ²½μ° Next.jsλ₯Ό μ μ©νλ κ³Όμ μμ μ¬λ¬ Hydration μ€λ₯λ₯Ό λ§λ¬λ€..
-   κ²°κ΅­ μλ²μμ λ λλ§μ μνν΄μΌ νλ―λ‘ CSRκ³Ό λ¬λ¦¬ λ³λμ μλ²κ° νμν΄μ§λ€.
-   TTVκ° λΉ λ₯΄λλΌλ λ²λ€λ§λ JS νμΌμ μΆκ°λ‘ λ°μ μΈν°λ μμ μ μ©νλ κ³Όμ μ΄ νμνλ―λ‘ TTIκ° μλμ μΌλ‘ λ¦μ΄μ§λ€.
-   μ΄λ‘ μΈν΄ νλ©΄μ λ³΄μ΄μ§λ§ μ¬μ©μκ° μνΈμμ© ν  μ μλ κΈ°κ°μ΄ μ‘΄μ¬νκΈ°μ, μ¬μ©μ μΈ‘λ©΄μμ ν¨μ¨μ΄ λ¨μ΄μ§λ€.

# βοΈ Single, Multiple Page Application

### 1. SPA (Single Page Application)

-   νλμ νμ΄μ§μμ μλ‘μ΄ νμ΄μ§λ₯Ό λΆλ¬μ€μ§ μκ³ , νμν λΆλΆλ§ λμ μΌλ‘ λ³κ²½νλ μ νλ¦¬μΌμ΄μμ **SPA** λΌκ³  ν¨.
-   μ΄κΈ°μ SPA λ AJAX ν΅μ μ νμ©νμ¬ νμν λ°μ΄ν°λ₯Ό λμ μΌλ‘ λ°μΈλ©νλ λ°©μμ΄μμ.
-   νμ§λ§ λͺ¨λ  κ³Όμ μ μμ AJAXλ‘ κ΅¬ννκΈ°μλ λλ¬΄λ λ§μ λΉμ©μ΄ λ€κΈ°μ, μ΄λ₯Ό λμμ£Όλ νλ μμν¬λ€μ΄ λλλμμ.
-   νμ¬λ **React, Vue, Angular** μ κ°μ νλ μμν¬κ° SPA μλΉμ€λ₯Ό μν΄ λ§μ΄ μ°μ΄κ³  μμ.

> **TTV? TTL? μ΄κ²λ€μ λ λ­κ°?**

-   TTV (Time To View) : μ¬μ©μκ° μ΄νλ¦¬μΌμ΄μ νλ©΄μ `"λ³΄κΈ°κΉμ§"` κ±Έλ¦¬λ μκ°.
-   TTI (Time To Interact) : μ¬μ©μκ° μ΄νλ¦¬μΌμ΄μ νλ©΄κ³Ό `"μνΈμμ©"` ν  μ μκΈ°κΉμ§ κ±Έλ¦¬λ μκ°.
-   μ¦ TTVκ° λΉ λ₯΄λ©΄ νλ©΄μ΄ λΉ λ₯΄κ² λ³΄μ΄λ κ²μ΄λ©°, TTIμ΄ λΉ λ₯΄λ€λ©΄ μΉμ λΉ λ₯΄κ² μ¬μ©ν  μ μλ€λ κ²μ΄λ€.

#### 1-1. SPAμ μ₯λ¨μ 

1. μ₯μ 

-   μ΄λ° λ λλ§ μ, μ νλ¦¬μΌμ΄μ κ°λμ νμν μ μ  νμΌμ λ°μμ€λ―λ‘ μ΄ν μλ‘κ³ μΉ¨μ ν  νμκ° μμ.
-   κ·Έλ κΈ°μ κΈ°μ‘΄ Static Web Pageμμ λκΌλ λ±λ± λκΈ°λ λͺ¨μ΅μ΄ λ³΄μ΄μ§ μμ, λΆλλ½κ³  μμ°μ€λ¬μ΄ μ¬μ©μ κ²½νμ λλΌκ² ν΄μ€.
-   TTVμ TTIκ° λμμ μμλκΈ°μ, μ¬μ©μλ νμ΄μ§κ° λ‘λ©λμλ§μ κ³§λ°λ‘ μ νλ¦¬μΌμ΄μμ μ¬μ©ν  μ μμ.
-   λν μ΄νμ λμμμ μΆκ°μ μΈ μ λ³΄κ° νμν κ²½μ°μλ νμν λ¦¬μμ€λ§ μμ²­νκΈ°μ, μλ²μ λΆλ΄μ΄ μλμ μΌλ‘ μ μ΄μ§λ€.
-   κΈ°μ‘΄μλ μλ²μμ λ λλ§ λ νμ΄μ§λ₯Ό λ°μλ€λ©΄, SPAμ κ²½μ° μ΅μ΄ μμ²­ μ΄ν λ΄λΆμμ λ λλ§μ νκΈ°μ μλ²μ λΆλ΄μ λΆμ°μν¨λ€.

2. λ¨μ 

-   μ΄κΈ°μ μ νλ¦¬μΌμ΄μ κ°λμ νμν λλΆλΆμ λ¦¬μμ€λ₯Ό λ°μμΌ νκΈ°μ, κ³§λ°λ‘ νμ΄μ§λ₯Ό λ‘λ©νμ§ λͺ»νλ€.
-   μ¦ TTV (Time To View) κ° μλμ μΌλ‘ λ¦μ. νμ΄μ§ λ λλ§ μ κΉμ§ μ¬μ©μλ ν λΉ νλ©΄λ§ λ°λΌλ³΄μμΌ ν¨.
-   μ΄λ₯Ό ν΄κ²°νκΈ° μν΄ λ§μ μμ JS μ½λλ₯Ό λ²λ€λ§ νμ¬ λΆν  μ κ³΅νλ Code Splitting κΈ°λ²μ΄ μμΌλ, κ·Όλ³Έμ μΈ ν΄κ²°μ±μ μλ.
-   SEO (Search Engine Optimization), μ¦ κ²μ μμ§ μ΅μ νλ₯Ό μ§ννκΈ° μ΄λ €μ. μλ²μλ μ΄κΈ° HTMLμ΄ λΉμ΄μλ κ²½μ°κ° λ§κΈ° λλ¬Έ.
-   μ¬μ©μμ μ λ³΄λ₯Ό ν΄λΌμ΄μΈνΈ μΈ‘μμ κ΄λ¦¬ν  κ²½μ°, λ³΄μ μ΄μκ° λ°μν  μ μμ. (μ μ₯ν  κ³΅κ°μ΄ μΏ ν€, μ€ν λ¦¬μ§ λΏ)

### 2. MPA (Multiple Page Application)

-   μ¬λ¬ κ°μ νμ΄μ§λ‘ μ΄λ£¨μ΄μ§ μ΄νλ¦¬μΌμ΄μμ΄λ©°, μλ‘μ΄ νμ΄μ§λ₯Ό μμ²­ν  λλ§λ€ μλ²μμ λ λλ§ λ μ μ  λ¦¬μμ€λ₯Ό μ λ¬ν¨.
-   νμ¬ νμ΄μ§μμ λ€λ₯Έ νμ΄μ§λ‘ μ΄λνκ±°λ, μλ‘κ³ μΉ¨μ΄ μ§νλ  κ²½μ° μλ²μμ νμ΄μ§λ₯Ό λ€μ λ λλ§ ν΄μΌ νλ€.

#### 2-1. MPAμ μ₯λ¨μ 

1. μ₯μ 

-   μλ²μμ μ¬μ μ λ λλ§ λ μ μ  λ¦¬μμ€λ₯Ό λ°μμΌλ‘μ λΉ λ₯΄κ² μ νλ¦¬μΌμ΄μ νλ©΄μ λμΈ μ μλ€.
-   μ¦ TTV κ° SPAμ λΉν΄ μλ±ν λΉ λ₯΄λ€. ν΄λΌμ΄μΈνΈμμλ μ¬μ μ λ λλ§λ νμ΄μ§λ₯Ό λμ°κΈ°λ§ νλ©΄ λκΈ° λλ¬Έμ΄λ€.
-   SEO (Search Engine Optimization), μ¦ κ²μ μμ§ μ΅μ νλ₯Ό μ§ννκΈ° μ¬μ, μλ²μμ μ¬μ μ λ§λ€μ΄μ§ HTMLμ μ λ¬λ°κΈ° λλ¬Έ.

2. λ¨μ 

-   νλ©΄μ λΉ λ₯΄κ² λμΈ μ μμΌλ, λμ μΈ μΈν°λ μμ λ΄λΉνλ JS νμΌμ μ μ©νκΈ°κΉμ§μ νμ΄ μμ.
-   μ¦ TTI (Time To Interact) κ° TTVλ³΄λ€ μλμ μΌλ‘ λ¦κ³ , κ·Έ μκ° λμ μ¬μ©μλ μ΄λ ν μΈν°λ μλ μ§νν  μ μμ
-   νμ΄μ§λ₯Ό μ΄λν  λλ§λ€ κ²°κ΅­ μλ²μμ μ¬μ μ λ λλ§ λ νμΌμ λ°μμΌ νκΈ°μ, λ§€ μμ²­λ§λ€ κΉλΉ‘μ νμμ΄ λ°μν¨.
-   μλ²μμ λ§€ μμ²­ μμ λ³΄λ΄μΌ ν  λ°μ΄ν°μ μ¬μ΄μ¦κ° μ»€μ§μΌλ‘μ, μλ² νΈλν½μ΄ μμΉν  κ°λ₯μ±μ΄ λμ.

# βοΈ SPA, MPA, CSR, SSR?

### 1. SPA = CSR? μ°¨μ΄μ μ λ­κΉ?

-   SPAμ CSRμ κ°μ κ°λμ΄ μλλ€. μ ννλ SPAλ₯Ό **κ΅¬ννκΈ° μν΄** CSR λ°©μμ΄ μ°μ΄λ κ²μ΄λ€.
-   `SPA vs MPA` λ μ νλ¦¬μΌμ΄μ κ΅¬λμ μν΄ νμ΄μ§λ₯Ό νλλ§ μ°λμ§, μ¬λ¬ κ°λ₯Ό μ°λμ§μ λν μ°¨μ΄λ€.
-   `CSR vs SSR` μ νμ΄μ§μ λ λλ§μ΄ ν΄λΌμ΄μΈνΈμμ μΌμ΄λλμ§, μλ²μμ μΌμ΄λλμ§μ λν μ°¨μ΄λ€.
-   λ°λΌμ SPAλ μ²« νμ΄μ§λ§ μλ²μμ λ°μμ€κ³  μ΄νμ λ³νλ λμ μΌλ‘ μ²λ¦¬νκΈ° μν΄ CSR λ°©μμ μ±ννλ€.
-   λ°λλ‘ MPAμ κ²½μ° νμ΄μ§κ° λ¬λΌμ§ λλ§λ€ μλ²μμ λ λλ§ λ λ¦¬μμ€λ₯Ό λ°μμ€κΈ° μν΄ SSR λ°©μμ μ±ννλ€.

### 2. SPAλ CSRλ‘, MPAλ SSRλ‘?

-   SPAλ SSRλ‘ κ΅¬νμ΄ μ΄λ ΅κ³ , MPAλ CSRλ‘ κ΅¬νμ΄ μ΄λ ΅λ€.
-   νμ§λ§ SPAμμ μ²« λ‘λ©μ κ²½μ°μλ§ SSRμ μ°κ³ , μ΄νμ μμ²­μ λν΄μλ CSR λ°©μμ μΈ μ μλ€. (Next.js)
-   Universal Rendering μ μ¬μ©ν  κ²½μ° μ΄κΈ° HTMLμ΄ λΉμ΄ μμ§ μμ SEOλ₯Ό μ΅μ ν ν  μ μλ€λ μ₯μ μ΄ μλ€.

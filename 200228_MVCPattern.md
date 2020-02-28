# ✏️

## MVC (Model View Controller) Pattern

* `Model` ? 데이터 관리 역할
* `View` ? 화면 관리 역할
* `Controller` ? Model 과 View를 연결 시켜 주는 역할



### Model

: **Data 를 관리하는 역할**

Database에 있는 데이터를 가져와서 또다른 객체에게 전달해주는 역할이며, 
외부 객체로 부터 입력 데이터를 받아서 database에 넣어주기도 한다.

FE에서는  database에 직접 접근하지 않고 API 형태로 접근한다. 
API 를 통해서 데이터를 가져오고 그 데이터를 다른 객체에 전달하는 역할.
외부 객체로 부터 데이터를 입력받으면 그 데이터를 backend API 를 통해서 호출하는 역할.



### View

: **데이터를 가지고 화면을 관리하는 역할**

`Model`의 데이터를 화면에 그리는 역할이다. (보통은 html, css, js로 구현되어 있음)

& 사용자가 입력하는 데이터를 처리하는 역할도 한다.
사용자가 화면을 통해 입력을 하게 되면, `View` 는 입력 이벤트를 받아서 입력된 값을 다른 객체에 전달하는 역할을 한다.



**=> Model과 View는 직접적으로 서로 연결되어 있지 않다. Model과 View를 연결시켜주는 역할을 하는게 Controller**  



### Controller

`Model` 로 부터 데이터를 가져오고 그 데이터를 `View` 에게 전달한다.

반대로, `View` 로부터 사용자의 입력 데이터를 얻고, 그 데이터를 ` Model` 에게 전달해주는 역할을 한다.



<img src="./img/MVC pattern-3.jpg" style="width: 500px !important; height: auto">




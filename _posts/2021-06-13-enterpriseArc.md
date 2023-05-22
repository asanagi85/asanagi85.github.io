
엔터프라이즈 아키텍처

-규모가 큰 어플을 개발할때
1)대형 솔루션, 프로그램 개발 사용하는 프로젝트 구성 방식(큰 틀을 구성)
2)다양한 플랫폼을 지원하는 재사용성이 높은 코드를 작성하는 방식

새로운 솔류션 작성후 4가지 폴더를 생성
BusinessLogicLayer
DataAccessLayer
PresentationLayer
Common

#클래스 라이브러리 종류
1) .net Frameword
2) .net core
3) .net Frameword(Portable) - Xamarin
4) .net Standard

예1)
asp.net (닷넷프레임워크)
data 클래스 라이브러리 (닷넷코어)


18강
의존성 주입(Dependency Injection)
-프로그래밍에서 구성요소간의 의존 관계가 소스코드 내부가 아닌 외부의 설정파일
등을 통해 정의되게 하는 디자인 패턴

IoC (Iversion of Control)
제어 반전
- 프로그래머가 작성한 프로그램이 재사용 라이브러리의 흐름제어를 받게 되는 소프트웨어 
디자인 패턴을 말한다.

IoC Container
- 객체를 프레임워크에서 간접적으로 생성, 소멸 시켜주는 컨테이너를 뜻함.

19강
객체를 라이브러리화 해서 여기저기서 공유하면서 사용하는 방법(필요 할때마다 인스턴스를 생성할 필요가 없음)

순서
1. 솔루션 만들기
2. Common, BusinessLogicLayer, DataAccessLayer, PresentationLayer의 폴더를 만들기
3. Common에 Class Library(.Net Framework)생성 > 딸려오는 클래스를 가장 기초가 되는 vo로 만들기(geter/seter)
4. DataAccessLayer에 Class Library(.Net Framework)생성 > 딸려오는 클래스는 이름에 I+vo 명으로 해서 인터페이스로 만들어주기
5. 같은 DataAccessLayer에 Class Library(.Net Framework)생성(이름은 실제로 사용할 db종류이름을 넣을것 예를들어 MySql을 사용한다면 (솔루션명).MSSQL.DAL) > 딸려오는 클래스에는 4번에서 만든 인터페이스를 상속시키자
6. BusinessLogicLayer에 Class Library(.Net Framework)생성(이름은 솔루션명.BLL) >
딸려오는 클래스에는  vo명 + Bll으로 명명
7. private readonly IUserDal _userDal;을 선언 > 생성자 만들기 > 임시적으로 4번 인터페이스 임플리먼트 시켜서 메소드들 추가 > 상속 지우기 > null또는 없는값들 throw처리 > return 값들 정의
8. 솔루션의 누겟패키지 > Unity.MVC를 추가 > PresentationLayer의 App_Start.UnityConfig클래스에 RegisterTypes를 등록 > 
    container.RegisterType<UserBll>();
    container.RegisterType<IUserDal, UserDal>(); 추가
9. Controller폴더에 HomeController.cs를 만들고 
 private readonly UserBll _userBll;

        public HomeController(UserBll userBll)
        {
            _userBll = userBll;
        }
        public ActionResult Index()
        {
            var list = _userBll.GetUserList();
            return View();
        } 를 추가
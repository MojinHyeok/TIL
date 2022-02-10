## **Function**

1. 원칙

- 한가지 일만 해야한다.
- identiation, while, nested if 등은 없어야..
- 잘 지어진 서술적인 긴 이름을 갖는 많은 /작은 함수들로 유지해야한다.
  - smll many function 
  - nice descriptive long name

2. The First Rule of Functions

-  더이상 작아질 수 없을 만큼 작아야 한다.
- **큰 함수를 보면 클래스로 추출할 생각을 해야한다.**
- 클래스는 일련의 변수들에 동작하는 기능의 집합.

함수를 작게 만들어야 하는 이유는 크게 3가지입니다.

- 읽기가 쉬워진다
- 이해하기 쉬워진다
- 함수가 어떠한 의도로 만들었는지 잘 나타낸다

리팩토링 예제: [https://github.com/msbaek/fitness-example](https://github.com/msbaek/fitness-example%EB%A5%BC)

를 참고하시면 됩니다. 리팩토링 방법은 아래와 같습니다.

![img](https://blog.kakaocdn.net/dn/bTAh1V/btrssBNQ2qA/QthxQQDJ73AH2Gz1acrXhk/img.png)

리팩토링 전 코드

```
public class FitnessExample {
    public String testableHtml(PageData pageData, boolean includeSuiteSetup) throws Exception {
        WikiPage wikiPage = pageData.getWikiPage();
        StringBuffer buffer = new StringBuffer();

        if (pageData.hasAttribute("Test")) {
            if (includeSuiteSetup) {
                WikiPage suiteSetup = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
                if (suiteSetup != null) {
                    WikiPagePath pagePath = wikiPage.getPageCrawler().getFullPath(suiteSetup);
                    String pagePathName = PathParser.render(pagePath);
                    buffer.append("!include -setup .").append(pagePathName).append("\n");
                }
            }
            WikiPage setup = PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
            if (setup != null) {
                WikiPagePath setupPath = wikiPage.getPageCrawler().getFullPath(setup);
                String setupPathName = PathParser.render(setupPath);
                buffer.append("!include -setup .").append(setupPathName).append("\n");
            }
        }

        buffer.append(pageData.getContent());
        if (pageData.hasAttribute("Test")) {
            WikiPage teardown = PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
            if (teardown != null) {
                WikiPagePath tearDownPath = wikiPage.getPageCrawler().getFullPath(teardown);
                String tearDownPathName = PathParser.render(tearDownPath);
                buffer.append("!include -teardown .").append(tearDownPathName).append("\n");
            }
            if (includeSuiteSetup) {
                WikiPage suiteTeardown = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage);
                if (suiteTeardown != null) {
                    WikiPagePath pagePath = wikiPage.getPageCrawler().getFullPath(suiteTeardown);
                    String pagePathName = PathParser.render(pagePath);
                    buffer.append("!include -teardown .").append(pagePathName).append("\n");
                }
            }
        }

        pageData.setContent(buffer.toString());
        return pageData.getHtml();
    }
}
```

리팩토링 후

```
public class FitnessExample {
    public String testableHtml(PageData pageData, boolean includeSuiteSetup) throws Exception {
        return new TestableHtmlBuilder(pageData, includeSuiteSetup).invoke();
    }

    private class TestableHtmlBuilder {
        private PageData pageData;
        private boolean includeSuiteSetup;
        private WikiPage wikiPage;
        private StringBuffer buffer;

        public TestableHtmlBuilder(PageData pageData, boolean includeSuiteSetup) {
            this.pageData = pageData;
            this.includeSuiteSetup = includeSuiteSetup;
            buffer = new StringBuffer();
            wikiPage = pageData.getWikiPage();
        }

        public String surround() throws Exception {
            if (isTestPage()) {
                includeSetUps();
                buffer.append(pageData.getContent());
                includeTearDowns();
            }
            pageData.setContent(buffer.toString());
            return pageData.getHtml();
        }

        private boolean isTestPage() throws Exception {
            return pageData.hasAttribute("Test");
        }

        private void includeTearDowns() throws Exception {
            includeInherited("teardown", "TearDown");
            if (includeSuiteSetup)
                includeInherited("teardown", SuiteResponder.SUITE_TEARDOWN_NAME);
        }

        private void includeSetUps() throws Exception {
            if (includeSuiteSetup)
                includeInherited("setup", SuiteResponder.SUITE_SETUP_NAME);
            includeInherited("setup", "SetUp");
        }

        private void includeInherited(String setup1, String suiteSetupName) throws Exception {
            WikiPage suiteSetup = PageCrawlerImpl.getInheritedPage(suiteSetupName, wikiPage);
            if (suiteSetup != null) {
                includePage(setup1, suiteSetup);
            }
        }

        private void includePage(String teardown1, WikiPage suiteTeardown) throws Exception {
            WikiPagePath pagePath = wikiPage.getPageCrawler().getFullPath(suiteTeardown);
            String pagePathName = PathParser.render(pagePath);
            buffer.append("!include -" + teardown1 + " .").append(pagePathName).append("\n");
        }
    }
```

주로 Extract Method 의 과정과 변수명들을 통합 시키는 과정을 진행하였습니다.

하지만 리팩토링 후에도 testtableHTML()는 하나이상의 일을 하는 것처럼 보이는데

1.테스트페이지 인지 결정

2.setups,teardowns를 include

3.HTML페이지 렌더링

이 세 가지 일을 하는 메서드의 이름은 surround다. surrond는 이 세가지 동작의 추상화된 이름인데 이 세 가지 일을 surround라는 하나의 일로 추상화 했기 때문에 세가지 일을 한다고 볼 수 없다고 합니다. 

예시를 설명해주신것이 로그인 부분인데

로그인의 경우

아이디와 비밀번호를 입력받는다, 과정 하나 

DB에있는 정보를대조한다. 과정 둘 해서 두 가지 과정인데 로그인이라는 하나의 일로 추상화 할 수 있습니다.
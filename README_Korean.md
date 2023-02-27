# code-review-tips(한국어 번역)

## 목차

1. [개요](##개요)
2. [왜 코드 리뷰를 해야 하는가?](##왜 코드 리뷰를 해야 하는가?)
3. [기초](##기초)
4. [가독성](##가독성)
5. [사이드 이펙트](##사이드 이펙트)
6. [임계치](##임계치)
7. [보안](##보안)
8. [성능](##성능)
9. [테스트](##테스트)
10. [기타 등등](##기타 등등)

## 개요

코드 리뷰는 리뷰어와 리뷰 대상 모두에게 공포를 불러일으킬 수 있다. 당신의 코드가 분석되는 것은 당신으로 하여금 불편함과 침입 받는다는 기분을 느끼도록 할 수 있다. 설상가상으로, 다른 사람의 코드를 검토하는 것은 문제를 찾고 어디서부터 시작해야 할 지조차 모르는 고통스럽고 모호한 작업처럼 느껴질 수 있다.

이 프로젝트는 당신과 당신의 팀이 작성한 코드를 검토하는 방법에 대한 몇 가지 확실한 팁을 제공하는 것을 목표로 한다. 모든 예제는 JavaScript로 작성되었지만 조언은 모든 언어의 모든 프로젝트에 적용할 수 있어야 한다. 완벽한 목록은 아니지만, 사용자가 기능을 보기 훨씬 전에 가능한 한 많은 버그를 잡는 데 도움이 되기를 바란다.

## 왜 코드 리뷰를 해야 하는가?

코드 리뷰는 소프트웨어 엔지니어링 프로세스에서 필수적이다. 작성한 코드에서 모든 문제를 혼자서 파악할 수 없기 때문이다. 그래도 괜찮다! 세계 최고의 농구 선수들도 슛을 놓친다.

다른 사람들에게 우리의 작업을 리뷰하게 하면 가능한 한 에러를 최소화 한 최고의 제품을 유저들에게 제공할 수 있다. 꼭 당신의 팀에서 새롭게 추가되는 코드를 위한 코드 리뷰 프로세스를 확립하길 바란다. 당신과 당신의 팀에 알맞은 프로세스를 찾아라. 완벽한 단 하나의 방법이란 건 없다. 중요한 것은 최대한 정기적으로 코드 리뷰를 진행하는 것이다.

## 기초

### 코드 리뷰는 최대한 자동화되어야 한다.

정적 분석 도구로 처리될 수 있는 디테일들에 대해 논의하는 것을 피해라. code formatting이나 let, var 사용에 대해 다투지 마라. formatter와 linter를 사용하는 것은 컴퓨터가 당신과 당신의 팀을 위해 해줄 수 있는 리뷰로부터 시간을 절약해 줄 것이다.

### API에 대한 논의는 피해야 한다.

이에 대한 논의는 코드가 써지기도 전에 했어야 한다. 콘크리트를 이미 부었다면, 기초 공사에 대해 논의하지 마라.

### 코드 리뷰는 친절해야 한다.

당신의 코드를 리뷰 받는 것은 무서운 일이며, 경험이 풍부한 개발자조차도 불안한 일이다. 긍정적인 언어를 쓰고, 당신의 동료들로 하여금 일터에서 안전하고 편안하게 느끼도록 만들어 줘라.

## 가독성

### 오타는 수정되어야 한다.

트집 잡기는 최소화하고, linter, compiler, formatter의 몫으로 남겨둬라. 오타가 있는 경우와 같이 할 수 없는 경우 수정을 제안하는 친절한 댓글을 남겨라. 때때로 큰 차이를 만드는 것은 작은 것이다!

### 변수 및 함수 이름은 명확해야 한다.

이름 짓기는 컴퓨터 과학에 있어서 가장 어려운 문제들 중 하나다. 우리는 모두 변수, 함수 그리고 파일들에 헷갈리는 이름을 주곤 한다. 만약 어떤 이름을 읽었는데 이해가 되지 않는다면, 보다 명확한 이름을 제안함으로써 동료들을 도와 주어라.

```javascript
// This function could be better named as namesToUpperCase
function u(names) {
  // ...
}
```

### 함수의 길이는 짧아야 한다.

함수는 한 가지 일만 해야 한다! 함수가 길다면 보통 너무 많은 일들을 하고 있는 것이다. 동료에게 함수를 여러 개의 짧은 함수로 분리하라고 말하라.

```javascript
// This is both emailing clients and deciding which are active. Should be
// 2 different functions.
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

### 파일들은 응집성 있으며 이상적으로 짧아야 한다. 

함수와 같이, 파일은 단 한 가지에만 관련되어 있어야 한다. 파일은 하나의 모듈을 대표하며, 모듈은 당신의 코드에서 한 가지 일을 해야 한다.

예를 들어, 만약 당신의 모듈의 이름이 `fake-name-generator` 이라면, 그것은 가짜 이름을 만들어내는 데 책임이 있어야 한다. 만약 이 모듈이 DB에 있는 이름들에 대한 query 등을 포함하고 있다면, 그것은 또 다른 분리된 모듈이어야 한다.

파일의 길이가 어느 만큼이어야 한다는 규칙은 없다. 그러나, 아래와 같이 길고 **서로와 관련 없는 함수들을 포함**하고 있다면 분리되어야 한다.

```javascript
// Line 1
import _ from 'lodash';
function generateFakeNames() {
  // ..
}

// Line 1128
function queryRemoteDatabase() {
  // ...
}
```

### export된 함수들은 문서화 되어야 한다.

만약 당신의 함수를 라이브러리처럼 사용하도록 의도했다면, 그 함수를 사용할 사람들을 위해서 함수가 어떤 역할을 하는지를 주석으로 남겨 놓는 것이 도움이 된다.

```javascript
// This needs documentation. What is this function for? How is it used?
export function networkMonitor(graph, duration, failureCallback) {
  // ...
}
```

### 복잡한 코드에는 주석이 있어야 한다.

만약 이름을 적절하게 지었는데도 로직이 아직 혼란스럽다면, 이제 주석을 달 시간이다.

```javascript
function leftPad(str, len, ch) {
  str = str + '';
  len = len - str.length;

  while (true) {
    // This needs a comment, why a bitwise and here?
    if (len & 1) pad += ch;
    // This needs a comment, why a bit shift here?
    len >>= 1;
    if (len) ch += ch;
    else break;
  }

  return pad + str;
}
```

## 사이드 이펙트

### 함수들은 가능한 한 순수해야 한다.

```javascript
// Global variable is referenced by the following function.
// If we had another function that used this name, now it'd be an array and it
// could break it. Instead it's better to pass in a name parameter
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();
```

### I/O 함수들은 실패 사례를 처리할 수 있어야 한다.

I/O 작업을 하는 모든 함수는 무언가 잘못 됐을 경우를 처리해야 한다.

```javascript
function getIngredientsFromFile() {
  const onFulfilled = buffer => {
    let lines = buffer.split('\n');
    return lines.map(line => <Ingredient ingredient={line} />);
  };

  // What about when this is rejected because of an error? What do we return?
  return readFile('./ingredients.txt').then(onFulfilled);
}
```

## 임계치

### Null에 대해 처리할 수 있어야 한다.

당신이 리스트 컴포넌트를 가지고 있다고 해보자. 모든 것이 좋아 보인다. 당신의 사용자들도 그것을 좋아하며, 당신은 승진을 하게 되었다! 그런데, 만약 어떤 데이터도 응답해오지 않는다면 어떤 일이 일어날까? null cases에는 무엇을 보여 줄 것인가? 당신의 코드는 발생할 수 있는 모든 경우에 대해 탄력적이어야 한다. 만약 당신의 코드에 어떤 나쁜 일이 일어날 가능성이 있다면, 결국 그것은 일어 나고야 만다.

```javascript
class InventoryList {
  constructor(data) {
    this.data = data;
  }

  render() {
    return (
      <table>
        <caption>Inventory</caption>
        <thead>
           <tr>
            <th scope="col">ID</th>
            <th scope="col">Product</th>
           </tr>
        </thead>
        <tbody>
          // We should show something for the null case here if there's // nothing
          in the data inventory
          {Object.keys(this.data.inventory).map(itemId => (
            <tr key={i}>
              <td>{itemId}</td>

              <td>{this.state.inventory[itemId].product}</td>
            </tr>
          ))}
        </tbody>
      </table>
    );
  }
}
```

### 아주 큰 케이스들을 처리하라 

위의 리스트에 1만 개의 아이템들이 응답해 오면 어떤 일이 일어날까? 이 경우, 당신은 pagination이나 무한 스크롤이 필요할 것이다. 볼륨과 관련된 잠재적인 edge-case들에 대해 항상 평가하라. 특히 UI 개발에 있어서.

### 단일한 케이스들을 처리하라

```javascript
class MoneyDislay {
  constructor(amount) {
    this.amount = amount;
  }

  render() {
    // What happens if the user has 1 dollar? You can't say plural "dollars"
    return (
      <div className="fancy-class">
        You have {this.amount} dollars in your account
      </div>
    );
  }
}
```

### 사용자 입력에는 제한이 있어야 한다.

사용자들은 당신에게 무제한의 데이터를 입력해 보낼 수도 있다. 사용자 데이터를 다루는 함수라면 제한을 거는 것이 중요하다.

```javascript
router.route('/message').post((req, res) => {
  const message = req.body.content;

  // What happens if the message is many megabytes of data? Do we want to store
  // that in the database? We should set limits on the size.
  db.save(message);
});
```

### 함수들은 예상치 못한 사용자 입력을 처리해야 한다

사용자들은 항상 데이터 입력으로 당신을 놀라게 할 것이다. 항상 올바른 데이터, 또는 데이터조차 올 것을 기대하지 마라. [또한, 클라이언트에서의 검증에만 기대서도 안된다.](https://twitter.com/ryconoclast/status/885523459748487169)

```javascript
router.route('/transfer-money').post((req, res) => {
  const amount = req.body.amount;
  const from = user.id;
  const to = req.body.to;

  // What happens if we got a string instead of a number as our amount? This
  // function would fail
  transferMoney(from, to, amount);
});
```

## 보안

데이터 보안은 당신의 서비스에서 가장 중요한 요소다. 만약 사용자가 데이터 보안에 있어서 당신을 신뢰하지 못한다면, 당신은 비즈니스를 일굴 수 없을 것이다. 런타임 환경, 언어의 종류에 따라 수많은 보안에 관련된 괴롭힘이 당신의 애플리케이션을 힘들게 할 수 있다. 아래는 보안 문제에 대한 아주 작고 완벽하지 않은 리스트다. 절대 이 리스트에만 의존해서는 안 된다! 커밋할 때마다 최대한 많은 보안 검토를 자동화하고 일상적인 보안 감사를 수행하라.

### XSS는 허용되어서는 안 된다.

Cross-site scripting (XSS)는 웹 애플리케이션 보안 공격의 가장 큰 벡터 중 하나다. 당신이 유저의 데이터를 적절하게 정리하지 않고 당신의 페이지에 포함할 때 발생한다. 이것은 당신의 웹사이트가 원격 페이지들에서 소스 코드를 실행하도록 한다.

```javascript
function getBadges() {
  let badge = document.getElementsByClassName('badge');
  let nameQueryParam = getQueryParams('name');

  /**
   * What if nameQueryParam was `<script>sendCookie(document.cookie)</script>`?
   * If that was the query param, a malicious user could lure a user to click a
   * link with that as the `name` query param, and have the user unknowingly
   * send their data to a bad actor.
   */
  badge.children[0].innerHTML = nameQueryParam;
}
```

### Personally Identifiable Information (PII)는 유출되어서는 안 된다.

당신은 유저의 데이터를 가져올 때마다 큰 책임을 부여 받게 된다. URL, 분석 추적에서 데이터를 제삼자에게 유출하거나 액세스 권한이 없는 직원에게 데이터를 노출하면 사용자와 비즈니스에 큰 피해를 준다. 다른 사람들의 생명을 소중하게 생각하자!

```javascript
router.route('/bank-user-info').get((req, res) => {
  const name = user.name;
  const id = user.id;
  const socialSecurityNumber = user.ssn;

  // There's no reason to send a socialSecurityNumber back in a query parameter
  // This would be exposed in the URL and potentially to any middleman on the
  // network watching internet traffic
  res.addToQueryParams({
    name,
    id,
    socialSecurityNumber
  });
});
```

## 성능

### 함수들은 효율적인 알고리즘과 데이터 구조를 사용해야 한다.

경우마다 다르지만, 당신의 코드의 효율성을 증진할 수 있는 방법이 있는지 당신이 내릴 수 있는 최고의 판단을 내려라. 사용자들이 빠른 속도에 대해 당신에게 고마워 할 것이다.

```javascript
// If mentions was a hash data structure, you wouldn't need to iterate through
// all mentions to find a user. You could simply return the presence of the
// user key in the mentions hash
function isUserMentionedInComments(mentions, user) {
  let mentioned = false;
  mentions.forEach(mention => {
    if (mention.user === user) {
      mentioned = true;
    }
  });

  return mentioned;
}
```

### 중요한 작업들은 기록되어야 한다

기록은 사용자 행동에 대한 인사이트와 성능에 대한 측정 도구를 준다. 모든 작업이 기록되어야 하는 것은 아니나, 당신의 팀과 함께 데이터 분석에 대한 추적이 합리적인지 결정하라. 그리고 개인 식별 정보가 노출되지 않도록 하라!

```javascript
router.route('/request-ride').post((req, res) => {
  const currentLocation = req.body.currentLocation;
  const destination = req.body.destination;

  requestRide(user, currentLocation, destination).then(result => {
    // We should log before and after this block to get a metric for how long
    // this task took, and potentially even what locations were involved in ride
    // ...
  });
});
```

## 테스트

### 새로운 코드는 테스트해야 한다.

모든 새로운 코드는 테스트를 포함해야 한다. 버그를 고치는 코드든, 새로운 기능이든. 만약 버그 수정 코드라면, 버그가 고쳐졌음을 입증하는 테스트가 있어야 한다. 만약 새로운 기능이라면, 모든 컴포넌트에 유닛 테스트가 있어야 하며 기능이 다른 나머지 시스템과 함께 잘 작동하는지를 보장하는 통합 테스트가 있어야 한다.

### 테스트들은 해당 기능이 수행하는 모든 기능들을 테스트해야 한다

```javascript
function payEmployeeSalary(employeeId, amount, callback) {
  db.get('EMPLOYEES', employeeId)
    .then(user => {
      return sendMoney(user, amount);
    })
    .then(res => {
      if (callback) {
        callback(res);
      }

      return res;
    });
}

const callback = res => console.log('called', res);
const employee = createFakeEmployee('john jacob jingleheimer schmidt');
const result = payEmployeeSalary(employee.id, 1000, callback);
assert(result.status === enums.SUCCESS);
// What about the callback? That should be tested
```

### 테스트들은 함수의 한계와 edge-case들을 강조해야 한다.

```javascript
function dateAddDays(dateTime, day) {
  // ...
}

let dateTime = '1/1/2017';
let date1 = dateAddDays(dateTime, 5);

assert(date1 === '1/6/2017');

// What happens if we add negative days?
// What happens if we add fractional days: 1.2, 8.7, etc.
// What happens if we add 1 billion days?
```

## 기타 등등

> _“모든 것은 기타 등등으로 제출할 수 있다”_

> 조지 버나드 쇼

### TODO 주석들은 추적되어야 한다.

TODO 주석은 당신과 당신의 동료들에게 나중에 어떤 것들이 수정되어야 하는지를 알려 주는 데 매우 좋다. 때때로 당신은 코드를 전송하고 나중에 수정할 날을 기다려야 한다. 하지만 결국에는 꼭 그것을 정리해야 한다! 그렇기 때문에 당신은 그것들을 추적하고 이슈 추적 시스템에 일치하는 ID 값을 부여해 스케줄을 관리하고 추적을 지속해야 한다.

### 커밋 메시지들은 명료해야 하며 새로운 코드를 정확하게 설명해야 한다.

우리 모두는 “빌어먹을 것을 변경함” “망할” “으 하나만 더 고치면 됨”과 같은 커밋 메시지들을 작성한 적이 있다. 이것들은 웃기기도 하고 만족스럽다. 그러나 토요일 아침에는 도움이 되지 않는다. 왜냐하면 당신이 금요일 밤에 코드를 push 했고, git blame을 해도 그 나쁜 코드가 무엇을 했었는지 알 수 없기 때문이다. 코드를 정확하게 설명하는 커밋 메시지를 써라. 그리고 당신의 이슈 추적 시스템의 티켓 번호를 포함하라. 당신이 커밋 로그를 탐색하는 것을 훨씬 쉽게 만들어 줄 것이다.

### 코드는 해야 할 일을 해야 한다

이것은 명백해 보이나, 대부분의 리뷰어들은 시간이 없거나 매 번 수동 테스트를 하기 위해 시간을 쓴다. 모든 변경 사항의 비즈니스 로직이 설계에 따라 이루어지도록 하는 것이 중요하다. 코드에서 문제를 찾을 때 그것을 잊기 쉽다!

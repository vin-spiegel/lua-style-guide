
# 루아 스타일 가이드

   >루아 프로젝트에서 사용되는 스타일 가이드입니다

   1. [들여쓰기 및 서식](#indent)
   1. [주석](#annotation)
   1. [루아타입](#types)
   1. [테이블](#tables)
   1. [문자열](#strings)
   1. [함수](#functions)
   1. [변수](#variables)
   1. [조건식](#conditions)
   1. [블록](#blocks)
   1. [타입검사](#type-casting)
   1. [모듈](#modules)
   1. [객체지향](#oop)
   1. [테스팅](#testing)
   1. [파일구조](#files)
   1. [참고목록](#lists)

  
## <a name='indent'>들여쓰기 및 서식</a>

   * 들여쓰기에 탭과 공백을 함께 사용해서는 안 됩니다.

      ```lua
      for i, pkg in ipairs(packages) do
         for name, version in pairs(pkg) do
            if name == searched then
               print(version)
            end
         end
      end
      ```
   
   
   * `--` 뒤에 스페이스를 사용합니다

      ```lua
      --나쁜예
      -- 좋은예
      ```

   * 항상 쉼표와 연산자와 기호 뒤에 공백을 넣으세요

      ```lua
      -- 나쁜예
      local x = y*9
      local numbers={1,2,3}
      numbers={1 , 2 , 3}
      numbers={1 ,2 ,3}
      local strings = { "hello"
                     , "Lua"
                     , "world"
                     }
      dog.set( "attr",{
      age="1 year",
      breed="Bernese Mountain Dog"
      })

      -- 좋은예
      local x = y * 9
      local numbers = {1, 2, 3}
      local strings = {
         "hello",
         "Lua",
         "world",
      }
      dog.set("attr", {
         age = "1 year",
         breed = "Bernese Mountain Dog",
      })
      ```

   * 구문이 아닌 줄의 시작을 기준으로 테이블과 함수를 들여씁니다

      ```lua
      -- 나쁜예
      local my_table = {
                        "hello",
                        "world",
                     }
      using_a_callback(x, function(...)
                           print("hello")
                        end)

      -- 좋은예
      local my_table = {
         "hello",
         "world",
      }
      using_a_callback(x, function(...)
         print("hello")
      end)
      ```

   * 연결 연산자는 공백을 생략해도 됩니다

      ```lua
      -- okay
      local message = "Hello, "..user.."! This is your day # "..day.." in our platform!"
      ```

   * 선언이나 인수에서 함수 이름 뒤에 공백이 없어야 합니다

      ```lua
      -- 나쁜예
      local function hello ( name, language )
         -- 코드
      end

      -- 좋은예
      local function hello(name, language)
         -- 코드
      end
      ```

   * 함수 사이에는 빈 줄을 추가해주세요

      ```lua
      -- 나쁜예
      local function foo()
         -- 코드
      end
      local function bar()
         -- 코드
      end

      -- 좋은예
      local function foo()
         -- 코드
      end

      local function bar()
         -- 코드
      end
      ```

   * 변수 선언을 정렬하지 마세요

      ```lua
      -- 나쁜예
      local a               = 1
      local long_identifier = 2

      -- 좋은예
      local a = 1
      local long_identifier = 2
      ```

      > **해석:** 파일 어노테이션을 활용할때 `git blame` 에 노이즈를 추가시킵니다

   * 논리적 대응이 강조되어야 할 때는 정렬을 때때로 사용합니다

      ```lua
      -- okay
      sys_command(form, UI_FORM_UPDATE_NODE, "a",      FORM_NODE_HIDDEN,  false)
      sys_command(form, UI_FORM_UPDATE_NODE, "sample", FORM_NODE_VISIBLE, false)
      ```

## <a name='annotation'>주석</a>

   * [LDoc](https://stevedonovan.github.io/ldoc/) 스타일을 사용하여 문서화합니다. 각 매개변수 또는 반환 값 뒤에 입력 정보를 지정하는 것이 좋습니다

      ```lua
      -- 유닛이 공격을 할 때마다 발동되는 함수
      ---@param attacker userdata 공격을 한 유닛
      ---@param damaged_unit userdata 공격을 받은 유닛
      ---@param dmg integer 공격시 대미지
      ---@return integer 대미지
      function observer.on_attack(attacker, damaged_unit, dmg)
         -- 코드
      end
      ```

   * 주석에 `TODO` 및 `FIXME` 태그를 사용해 보세요. `TODO` 는 나중에 구현될 누락된 기능을 나타냅니다. `FIXME` 는 기존 코드의 문제점 (비효율적인 구현, 버그, 불필요한 코드 등)을 나타냅니다

      ```lua
      -- TODO: 메소드 삽입하기
      local function something()
         -- FIXME: 조건식 확인
      end
      ```

   * 인라인 주석보다 `LDoc` 스타일의 주석이 많이 쓰입니다

## <a name='types'>루아타입</a>

  - **기본 유형**: 기본 유형에 액세스하면 해당 값에 대해 직접 대입합니다

    + `string`
    + `number`
    + `boolean`
    + `nil`

      ```lua
      local foo = 1
      local bar = foo

      bar = 9

      print(foo, bar) -- => 1	9
      ```

  - **복합유형**: 복합 유형에 액세스할 때 해당 값에 대한 참조 작업을 수행합니다

    + `table`
    + `function`
    + `userdata`

      ```lua
      local foo = { 1, 2 }
      local bar = foo

      bar[0] = 9
      foo[1] = 3

      print(foo[0], bar[0]) -- => 9   9
      print(foo[1], bar[1]) -- => 3   3
      print(foo[2], bar[2]) -- => 2   2		
      ```

## <a name='tables'>테이블</a>

   * 테이블을 생성할 때 가능하면 해당 필드를 한 번에 모두 채우는 것이 좋습니다

      ```lua
      local player = {
         name = "Jack",
         class = "도적",
         hp = 100
      }
      ```

   * 가능한 위와 같이 선언하고, 식별자로 표현할 수 없는 키값을 사용할 때는 `["key"]` 구문을 사용합니다. 이 두 표기법이 한 테이블에서 겹치는 것은 피해주세요

      ```lua
      table = {
         ["1394-E"] = val1,
         ["UTF-8"] = val2,
         ["스킬종류"] = val2,
      }
      ```

   * 이미 알고 있는 테이블 값에 접근 할 때에는 `.` 표기법을 사용합니다

      ```lua
      local luke = {
         wizard = true,
         age = 28,
      }

      -- 나쁜예
      local is_wizard = luke["wizard"]

      -- 좋은예
      local is_wizard = luke.wizard
      ```

   * 변수가 있는 속성에 액세스하거나 테이블을 `list` 로 사용하는 경우 대괄호 표기법 `[]` 을 사용합니다.

      ```lua
      local vehicles = load_vehicles_from_disk("vehicles.dat")

      if vehicles["Porsche"] then
         porsche_handler(vehicles["Porsche"])
         vehicles["Porsche"] = nil
      end
      for name, cars in pairs(vehicles) do
         regular_handler(cars)
      end
      ```

## <a name='strings'>문자열</a>

   * 문자열에 "큰따옴표"를 사용하세요. 큰따옴표가 포함된 문자열을 작성할 때는 '작은따옴표'를 사용하세요.

      ```lua
      local name = "NekoLand"
      local sentence = 'The name of the program is "NekoLand"'
      ```

   * 80자가 넘어가는 문자열의 경우 연결 연산자 `..` 를 써야합니다

      ```lua
      -- bad
      local errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'

      -- bad
      local errorMessage = 'This is a super long error that \
      was thrown because of Batman. \
      When you stop to think about \
      how Batman had anything to do \
      with this, you would get nowhere \
      fast.'


      -- bad
      local errorMessage = [[This is a super long error that
      was thrown because of Batman.
      When you stop to think about
      how Batman had anything to do
      with this, you would get nowhere
      fast.]]

      -- good
      local errorMessage = 'This is a super long error that ' ..
      'was thrown because of Batman. ' ..
      'When you stop to think about ' ..
      'how Batman had anything to do ' ..
      'with this, you would get nowhere ' ..
      'fast.'
      ```

      > **해석:** 큰 따옴표는 많은 언어에서 문자열 구분 기호로 사용됩니다. 작은 따옴표는 이스케이프를 방지하는 데 유용합니다.



## <a name='functions'>함수</a>

   * [기능이 작은 함수는 건강에 이롭습니다](http://kiki.to/blog/2012/03/16/small-functions-are-good-for-the-universe/)
      ```lua
      -- 나쁜예
      function Turret:update(dt)
         -- 범위 내에서 가장 가까운 플레이어를 찾습니다
         local t
         local dx,dy,d
         local cd = math.huge
         for _,p in ipairs(Game.players)
            dx = self.x - p.x
            dy = self.y - p.y
            d = dx*dx + dy*dy
            if d < cd then
               t = p
               cd = d
            end
         end

         -- 플레이어에게 총알을 발사합니다
         if t then
            Bullet.new(self.x, self.y, math.atan2(self.x - t.x, self.y - t.y))
         end
      end
      ```

      ```lua
      -- 좋은예

      -- 업데이트 함수
      function Turret:update(dt)
         local target = self:getTarget()
         if target then self:shootAt(target) end
      end

      -- 타겟 구하기
      function Turret:getTarget()
         return self:getNearest(Game.players)
      end

      -- 가까운 타겟 구하기
      function Turret:getNearest(objects)
         local distance, nearest
         local shortestDistance = math.huge
         for _,object in ipairs(objects)
            distance = self:getSquaredDistance(object)
            if distance < shortestDistance then
               nearest = object
               shortestDistance = distance
            end
         end
         return nearest
      end

      -- 제곱 거리 구하기
      function Turret:getSquaredDistance(object)
         local dx, dy = self.x - object.x, self.y - object.y
         return dx*dx + dy*dy
      end

      -- 타겟에게 발사 하기
      function Turret:shootAt(target)
         Bullet.new(self.x, self.y, self:getAngle(target))
      end

      -- 각도 구하기
      function Turret:getAngle(object)
         return math.atan2(self.x - t.x, self.y - t.y)
      end
      ```

      이렇게 각각 한가지 기능만 하는 작은 함수들로 쪼개어진 클래스를 만들어 놓으면 아래와 같이 단 4줄로 서브클래스를 만들 수 있습니다

      ```lua
      otherTurret  =  class ( 'otherTurret' ,  Turret ) 
      function  otherTurret : getTarget () 
         return  self : getNearest (Game.asteroids) 
      end
      ```

   * 변수형 선언 보다 함수형 선언을 합니다. 이렇게 하면 익명 함수를 구분하는 데 도움이 됩니다

      ```lua
      -- 나쁜예
      local nope = function(name, options)
         -- ...stuff...
      end

      -- 좋은예
      local function yup(name, options)
         -- ...stuff...
      end
      ```

   * 루아 함수의 유효성 검사는 함수 안에서 최대한 빨리 검사하고 값 또한 되도록 빨리 반환시켜주는 것이 좋습니다

      ```lua
      -- 나쁜예
      local function is_good_name(name, options, arg)
         local is_good = #name > 3
         is_good = is_good and #name < 30

         -- ...stuff...

         return is_good
      end

      -- 좋은예
      local function is_good_name(name, options, args)
         if #name < 3 or #name > 30 then
            return false
         end

         -- ...stuff...

         return true
      end
      ```

   * 함수 호출시 문자열 인수가 1개라도 괄호를 생략하지 않습니다

      ```lua
      -- 나쁜예
      local data = get_data"Monster"..tostring(map_name)
      local data = get_data"Monster"

      -- 좋은예
      local data = get_data("Monster"..tostring(map_name))
      local data = get_data("Monster")..tostring(map_name)
      ```

   * 함수 인자로 테이블 을 1개만 받을 경우 괄호를 생략합니다

      ```lua
      -- 나쁜예
      local goblin = monster.new (
         {
            hp = 42,
            say = function()
               return "그르르......"
            end,
         }
      )

      -- 좋은예
      local goblin = monster.new {
         hp = 42,
         say = function()
            return "그르르......"
         end,
      }
      ```

      > **해석:** 위와 같이 한 개의 테이블을 인자로 받는 경우는 `우선 순위 문제` 가 발생하지 않으며 가독성이 좋아집니다

   * 모듈 및 클래스를 선언할 때 테이블 외부에 함수를 선언합니다

      ```lua
      -- 나쁜예
      local my_module = {
         a = function(x)
         -- 코드
         end
      }

      -- 좋은예
      local my_module = {}

      function my_module.a(x)
         -- 코드
      end
      ```

   * 메타 테이블을 선언할 때 메타 메서드는 테이블 내부에 정의해주세요

      ```lua
      -- 나쁜예
      local version_mt = {}
      version_mt.__call = function(a, b)
         -- 코드
      end

      -- 좋은예
      local version_mt = {
         __eq = function(a, b)
            -- 코드
         end,
         __lt = function(a, b)
            -- 코드
         end,
      }

      version_mt.method1 = function()
         -- 코드
      end
      ```

   




## <a name='variables'>변수</a>

   * 한 글자로 된 변수 이름은 피해야 합니다

   * 변수이름 `i` 는 `for` 문에서 카운팅 변수로만 사용해야 합니다

   * 키와 값을 가진 테이블을 순회할 때 `k` 및 `v` 보다 자세한 이름이 좋습니다

   * 무시되거나 안쓰는 변수에는 `_` 를 사용하세요

      ```lua
      for _, item in ipairs(items) do
         do_something_with_item(item)
      end
      ```

   * 변수 및 함수 이름은 주로 snake_case를 사용합니다

      ```lua
      -- 나쁜예
      local OBJEcttsssss = {}
      local thisIsMyObject = {}
      local c = function()
         -- ...stuff...
      end

      -- 좋은예
      local this_is_my_object = {}

      local function do_that_thing()
         -- ...stuff...
      end
      ```

      > **해석:** 표준 라이브러리는 소문자 이름이 결합된 소문자 API를 사용하지만 더 복잡한 API에서는 확장성이 좋지 않습니다. Snake_case는 표준 API와 함께 적절하게 보이는 경향이 있습니다

   * 객체 지향으로 코딩할 때 클래스는 `camelCase` 를 사용해야 합니다. 약어(예: HP)는 첫 번째 문자(Hp)만 대문자여야 합니다. 메소드는 `snake_case` 를 사용합니다

      ```lua
      for _, name in pairs(names) do
         -- ...stuff...
      end
      ```

   * `factory`에는 파스칼 케이스를 사용합니다

      ```lua
      -- bad
      local player = require('player')

      -- good
      local Player = require('player')
      local me = Player({ name = 'Jack' })
      ```

   * `boolean` 값을 리턴 하는 함수는 is_ 사용을 선호합니다.

      ```lua
      -- 나쁜예
      local function evil(alignment)
         return alignment < 100
      end

      -- 좋은예
      local function is_evil(alignment)
         return alignment < 100
      end
      ```

   * `UPPER_CASE` 오직 프로그램이 동작 중에는 변하지 않는 상수만 사용합니다
      ```lua
      local CONST = {
         IMPORTANT_KEY = 1
         IMPORTANT_KEY2 = 2
         IMPORTANT_KEY3 = 3
      }
      ```

   * `UPPER_CASE` 로 작명시 `_` 로 시작하는 대문자 이름을 사용하지 마세요. `lua` 의 예약어 규칙입니다
      ```lua
      -- 나쁜예
      local _MY_CONST_VAR_NUMBER = 101

      -- 좋은예
      local MY_CONST_VAR_NUMBER = 101
      ```

   * 변수를 선언할 때는 반드시 `local` 키워드를 사용하세요.

      ```lua
      -- 나쁜예
      superpower = get_superpower()

      -- 좋은예
      local superpower = get_superpower()
      ```

      > **해석:** `lua` 의 스크립팅 원칙은 기본적으로 전역 변수를 피해야합니다

   * 가능한 가장 작은 범위 단위로 변수를 할당하세요

   * 변수가 접근 가능한 범위의 맨 위에 변수를 선언하세요. 이렇게 하면 기존 변수를 더 쉽게 확인할 수 있습니다

      ```lua
      -- 나쁜예
      local bad = function()
         
         -- ...굉장히 긴 코드 ...

         -- ... 다른 긴 코드 ...

         local name = get_name()

         if name == "test" then
            return false
         end

         return name
      end

      -- 좋은예
      local function good()
         local name = get_name()

         -- ...굉장히 긴 코드 ...

         -- ... 다른 긴 코드 ...

         if name == "test" then
            return false
         end

         return name
      end
      ```

      > **해석:** `Lua` 에는 렉시컬 스코프가 있습니다. 변수의 범위가 좁을 수록 디버깅이 쉬워집니다

## <a name='conditions'>조건식</a>

   * `false` 및 `nil` 은 조건식에서 `거짓` 이며, 그 외의 자료형은 모두 `참`입니다
   
   * `false` 와 `nil` 의 차이를 알아야 하는 경우가 아니라면, `nil` 과 `false` 의 차이에 의존하는 `API` 를 설계하지 마세요

      ```lua
      -- 나쁜예
      if name ~= nil then
         -- ...stuff...
      end

      -- 좋은예
      if name then
         -- ...stuff...
      end
      ```

   * 대부분의 조건식 상황에서는 `false` 보다 `true` 를 확인하는 것이 좋습니다

      ```lua
      -- 나쁜예
      if not thing then
      -- ...stuff...
      end

      -- 좋은예
      if thing then
      -- ...stuff...
      end
      ```

   * 리턴 값으로 `nil` 을 내뱉는 함수 디자인을 피해주세요. 삼항 연산자로 `nil` 리턴을 막을수 있다면 적극 사용하는 것이 좋습니다

      ```lua
      -- 나쁜예
      local function normal_name(name)
         -- do something..
         return name
      end

      -- 좋은예
      local function default_name(name)
         -- name이 nil 이라면 "Spiegel"을 리턴한다
         return name or "Spiegel"
      end

      local function brew_coffee(machine)
         -- 조건식에 괄호를 이용하면 시각적으로 더 잘 보이게 할 수 있습니다
         return (machine and machine.is_loaded) and "커피 만드는중" or "물을 채워주세요"
      end
      ```

   * `else` 문 을 자제해 주세요. 변수 재할당을 막고, 코드를 간결하게 만들어줍니다

      ```lua
      -- 나쁜예
      local function full_name(first, last)
      local name

      if first and last then
         name = first .. ' ' .. last
      else
         name = 'John'
      end

      return name
      end

      -- 좋은예
      local function full_name(first, last)
      local name = 'John'

      if first and last then
         name = first .. ' ' .. last
      end

      return name
      end
      ```



## <a name='blocks'>블록</a>

   * 줄 길이에는 제한이 없습니다. 줄 길이는 한 줄에 하나의 명령문을 사용하여 자연스럽게 제한됩니다. 한 줄의 코드가 너무 긴 경우 (예: 256자 이상의 줄을 생성하는 표현식) 분리 하는 것이 좋습니다

   * `then break`, `break`, `return`, `람다식` 에만 한줄 블록을 사용합니다

      ```lua
      -- 좋은예
      if test then break end

      -- 좋은예
      if not ok then return nil, "다음과 같은 이유로 실패했습니다: " .. reason end

      -- 좋은예
      use_callback(x, function(k) return k.last end)

      -- 좋은예
      if test then
         return false
      end

      -- 나쁜예
      if test < 1 and do_complicated_function(test) == false or seven == 8 and nine == 10 then do_other_complicated_function() return end

      -- 좋은예
      if test < 1 and do_complicated_function(test) == false or seven == 8 and nine == 10 then
         do_other_complicated_function() 
         return false 
      end
      ```

   * 문장을 여러 줄로 분리하세요. 구분자의 용도로 세미콜론을 사용하지 마세요

      ```lua
      -- 나쁜예
      local whatever = "sure";
      a = 1; b = 2

      -- 좋은예
      local whatever = "sure"
      a = 1
      b = 2
      ```

## <a name='type-casting'>타입 검사</a>

   * 성능이 중요하지 않은 코드에서는 `assert()` 함수를 통한 형식검사가 좋습니다

      ```lua
      function manif.load_manifest(repo_url, lua_version)
         assert(type(repo_url) == "string")
         assert(type(lua_version) == "string" or not lua_version)

         -- ...
      end
      ```

   * 형식 변환시 표준 함수를 사용하세요

      ```lua
      -- 나쁜예
      local total_score = review_score .. ""

      local total_score = review_score + 0

      -- 좋은예
      local total_score = tostring(review_score)
      ```

## <a name='modules'>모듈</a>

   [모듈 작성 지침](http://hisham.hm/2014/01/02/how-to-write-lua-modules-in-a-post-module-world/)

   * 모듈을 `require` 할 변수는 항상 `local` 소문자 변수 이름이어야 합니다.

      ```lua
      local bar = require("foo.bar") 
      local goo = require("foo.goo")

      bar.say("hello") 
      goo.bye("Bye")
      ```

   * 임의로 모듈의 이름을 바꾸지 마십시오.

      ```lua
      -- 나쁜예
      local skt = require("socket")
      ```

      > **해석:** 어떤 모듈인지 확인하기 위해 맨 위로 계속 돌아가야 하는 경우 코드를 읽기가 훨씬 더 어렵습니다.

   * `local` 소문자 이름을 사용하여 모듈을 만들어주세요. `Ldoc` 스타일의 주석으로 `module` 이라고 표기할 수 있습니다

      ```lua
      --- @module foo.bar
      local bar = {}
      ```

   * 지역 변수와 충돌하지 않는 이름을 사용하세요. 예를 들어, `size` 와 같이 모듈 이름을 만들지 마세요

   * 모듈 안에서는 로컬 함수만 선언합니다. 모듈 내부의 함수는 외부에서 액세스할 수 없게 하는 것이 원칙입니다. 즉, `local function helper_foo()` 는 `helper_foo` 가 실제로 로컬임을 의미합니다.

   * 멤버 함수는 점 구문을 사용하여 모듈 테이블에 선언됩니다.

      ```lua
      function bar.say(greeting)
         print(greeting)
      end
      ```

   * 모듈에 전역을 설정하지 말고 항상 마지막에 테이블을 반환하십시오.

   * 모듈을 함수 처럼 사용하려면 모듈 테이블에 __call 메타메소드를 설정 하세요

      > **Rationale:** 모듈은 Lua 인터프리터 또는 기타 도구를 통해 내용을 검사할 수 있도록 테이블을 반환해야 합니다.

   * 모듈을 `require` 하면 다른 모듈을 로드하고 모듈 테이블을 반환하는 것 외에는 부작용이 발생하지 않아야 합니다.

   * 모듈에는 상태 값이나 변수가 없어야 합니다. 모듈에 구성이 필요한 경우 `factory` 에게 위임하세요.
   예를 들어 다음과 같이 만들지 마세요

      ```lua
      -- 나쁜예
      local my_player = require "player"
      my_player.set_str(50)
      
      -- 좋은예
      local Player = require("player")
      local my_player = Player.new({str = 50})

      -- 좋은예
      local player = require("player")
      local my_player = Player.new()
      -- 가져온 모듈이 클래스이고 생성된 객체가 인스턴스인 경우 setter나 getter를 쓸 수 있습니다
      my_player.set_str(50)
      ```

   * `require` 호출은 일반적인 `Lua` 함수 호출처럼 보여야 합니다.

      ```lua
      -- 나쁜예
      local bla = require "bla"

      -- 좋은예
      local bla = require("bla")
      ```

      > **해석:** 이것은 `require` 가 키워드가 아니라 함수 호출이라는 것을 암묵적으로 표현합니다. 다른 많은 언어가 위와 같은 문법을 따르므로 `require` 에 특별한 구문을 사용하면 `Lua` 를 처음 접하는 사람들을 곤경에 빠뜨릴 수 있습니다.

## <a name='oop'>객체 지향</a>

   * 다음과 같은 방법으로 클래스를 만듭니다.

      ```lua
      --- @module ServerScripts.myclass
      local myclass = {}

      local MyClass_mt = {}

      --- @class table
      local MyClass = {}
      setmetatable(MyClass, MyClass_mt)

      function MyClass:some_method()
         -- 코드
      end

      function MyClass:another_one()
         self:some_method()
         -- more 코드
      end

      function myclass.new()
         local self = {}
         setmetatable(self, { __index = MyClass })
         return self
      end

      return myclass
      ```

   * 클래스 테이블과 클래스 메타테이블은 모두 `local` 이어야 합니다. 메타메소드를 포함하는 경우 메타테이블은 MyClass_mt라는 최상위 로컬로 선언될 수 있습니다.

   > **해석:** 
   위의 코드에서 MyClass가 붙은 함수가 메서드라는 것을 쉽게 알 수 있습니다. 
   더 자세한 설계 근거는 [여기](http://hisham.hm/2014/01/02/how-to-write-lua-modules-in-a-post-module-world/)를 확인하세요

   * 메소드를 호출할 때 `:` 표기법을 사용하세요

      ```lua
      -- 나쁜예 
      my_object.my_method(my_object)
      -- 좋은예
      my_object:my_method()
      ```

      > **해석:** `:` 를 통한 호출은 암묵적으로 `객체지향` 스타일의 메서드를 나타냅니다


   * 메모리 이외의 리소스를 해제하기 위해 `__gc` 메타메서드에 의존하지 마세요. 객체가 파일과 같은 리소스를 관리하는 경우 해당 API에 `Destroy` 메서드를 추가하고 `__gc` 를 통해 자동으로 관리하지 마세요. `__gc` 를 통한 가비지 컬렉팅은 모듈의 사용이 끝나도 메모리가 유지될 확률이 높습니다. (표준 io 라이브러리는 이 권장 사항을 따르지 않으며 사용자는 종종 파일을 즉시 닫지 않으면 프로그램이 잠시 실행될 때 "열린 파일이 너무 많음" 오류가 발생할 수 있다는 사실을 잊어버립니다.)

      > **해석:** 가비지 컬렉터는 자동 메모리 관리를 수행합니다. 가비지 컬렉터가 언제 호출 될지에 대한 보장이 없으며 가비지 컬렉터가 실행되는 시점은 다른 리소스와 상관 관계가 없습니다.



## <a name = 'testing'>테스팅</a>

   * 실패할 수 있는 함수(예: `I/O` )는 오류 시 `nil` 및 문자열로 된 오류 메시지를 반환해야 하며, 그 뒤에 오류 코드와 같은 다른 반환 값이 올 수 있습니다.

   * `API` 를 잘못 사용했을때는 `error()` 또는 `assert()` 와 함께 오류를 발생 시켜야합니다

   * 루아로 만들어진 코드는 [luacheck](https://github.com/mpeterv/luacheck) 를 통한 검사에서 통과하는 것이 가장 좋습니다

   * 사용되지 않은 변수, 인수 또는 루프 변수는 사용되지 않는 변수가 명시적으로 추가된 경우 무시되어야 합니다. 
   
   * 일부 인수를 사용하지 않거나 인수가 가변적인 함수는 `_` 변수를 추가하는 대신 정해진 갯수의 인수를 받는 것이 좋습니다


   * `if` / `elseif` / `else` 블록의 시퀀스가 ​​`Switch` / `Case` 스타일의 목록을 구현하고 `Case` 중 하나가 `통과` 를 의미하는 경우 luacheck 경고 542(분기인 경우 비어 있음)도 무시할 수 있습니다. 예를 들어:


      ```lua
      if warning >= 600 and warning <= 699 then
         print("no whitespace warnings")
      elseif warning == 542 then
         -- pass
      else
         print("got a warning: "..warning)
      end
      ```

## <a name='files'>파일 구조</a>

   * `Lua` 파일의 이름은 모두 소문자로 지정해야 합니다.

   * `Lua` 파일은 최상위 `src` 디렉토리에 있어야 합니다. 메인 라이브러리 파일은 `modulename.lua` 라고 해야 합니다.

   * 테스트는 최상위 사양 디렉토리에 있어야 합니다. 테스트를 위해 [Busted](http://olivinelabs.com/busted/)같은 라이브러리를 사용할 수 있습니다

   * 실행 파일은 `src/bin` 디렉토리에 있어야 합니다.


   


## <a name='lists'>참고 목록</a>

   * https://github.com/luarocks/lua-style-guide
   * https://github.com/Olivine-Labs/lua-style-guide/
   * https://github.com/zaki/lua-style-guide
   * http://lua-users.org/wiki/LuaStyleGuide
   * http://sputnik.freewisdom.org/en/Coding_Standard
   * https://gist.github.com/catwell/b3c01dbea413aa78675740546dfd5ce2
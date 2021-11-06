
# 루아 스타일 가이드

이 스타일 가이드는 [LuaRocks](http://github.com/luarocks/luarocks) 프로젝트에서 사용되는 코딩 규칙을 나열합니다. 

참고 목록

* https://github.com/luarocks/lua-style-guide
* https://github.com/Olivine-Labs/lua-style-guide/
* https://github.com/zaki/lua-style-guide
* http://lua-users.org/wiki/LuaStyleGuide
* http://sputnik.freewisdom.org/en/Coding_Standard
* https://gist.github.com/catwell/b3c01dbea413aa78675740546dfd5ce2

## 들여쓰기 및 서식

* LuaRocks 스타일 에서는 3개의 공백으로 들여쓰기 합니다

```lua
for i, pkg in ipairs(packages) do
   for name, version in pairs(pkg) do
      if name == searched then
         print(version)
      end
   end
end
```

* 들여쓰기에 탭과 공백을 함께 사용해서는 안 됩니다.

## 주석

* [LDoc](https://stevedonovan.github.io/ldoc/) 스타일을 사용하여 문서화합니다. 각 매개변수 또는 반환 값 뒤에 입력 정보를 지정하는 것이 좋습니다.

```lua
--- Load a local or remote manifest describing a repository.
-- All functions that use manifest tables assume they were obtained
-- through either this function or load_local_manifest.
-- @param repo_url string URL or pathname for the repository.
-- @param lua_version string Lua version in "5.x" format, defaults to installed version.
-- @return table or (nil, string, [string]) A table representing the manifest,
-- or nil followed by an error message and an optional error code.
function manif.load_manifest(repo_url, lua_version)
   -- code
end
```

* 주석에 TODO 및 FIXME 태그를 사용해 보세요. TODO는 나중에 구현될 누락된 기능을 나타냅니다. FIXME는 기존 코드의 문제점(비효율적인 구현, 버그, 불필요한 코드 등)을 나타냅니다.

```lua
-- TODO: implement method
local function something()
   -- FIXME: check conditions
end
```

* 인라인 주석보다 `LDoc` 스타일의 주석이 많이 쓰입니다.

## 변수 이름

* 한 글자로 된 변수 이름은 피해야 합니다.

* 변수이름 `i` 는 `for` 문에서 카운팅 변수로만 사용해야 합니다

* 키와 값을 가진 테이블을 순회할 때 `k` 및 `v` 보다 자세한 이름이 좋습니다

* 무시되거나 안쓰는 변수에는 `_` 를 사용하세요.

```lua
for _, item in ipairs(items) do
   do_something_with_item(item)
end
```

* 변수 및 함수 이름은 주로 snake_case를 사용합니다.

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

> **해석:** 표준 라이브러리는 소문자 이름이 결합된 소문자 API를 사용하지만 더 복잡한 API에서는 확장성이 좋지 않습니다. Snake_case는 표준 API와 함께 적절하게 보이는 경향이 있습니다.

* 객체 지향으로 코딩할 때 클래스는 CamelCase를 사용해야 합니다. 약어(예: XML)는 첫 번째 문자(XmlDocument)만 대문자여야 합니다. 메소드는 snake_case를 사용합니다.

```lua
for _, name in pairs(names) do
   -- ...stuff...
end
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

* `UPPER_CASE` 오직 상수만 사용합니다

* `_` 로 시작하는 대문자 이름을 사용하지 마세요. lua의 예약어 규칙입니다

## 테이블

* 테이블을 생성할 때 가능하면 해당 필드를 한 번에 모두 채우는 것이 좋습니다

```lua
local player = {
   name = "Jack",
   class = "Rogue",
}
```

* 가능한 위와 같은 구문을 사용하고, 식별자로 표현할 수 없는 이름을 사용할 때는 ["key"] 구문을 사용합니다, 선언시 이 두 표기법이 겹치는 것은 피해주세요


```lua
table = {
   ["1394-E"] = val1,
   ["UTF-8"] = val2,
   ["and"] = val2,
}
```

## 문자열

* 문자열에 "큰따옴표"를 사용하세요. 큰따옴표가 포함된 문자열을 작성할 때는 '작은따옴표'를 사용하세요.

```lua
local name = "LuaRocks"
local sentence = 'The name of the program is "LuaRocks"'
```

> **해석:** 큰 따옴표는 많은 수의 프로그래밍 언어에서 문자열 구분 기호로 사용됩니다. 작은 따옴표는 이스케이프를 방지하는 데 유용합니다.

## 줄 길이

* 줄 길이에는 제한이 없습니다. 줄 길이는 한 줄에 하나의 명령문을 사용하여 자연스럽게 제한됩니다. 여전히 너무 긴 줄이 생성되는 경우(예: 256자 이상의 줄을 생성하는 표현식) 이는 표현식이 너무 복잡하고 합리적인 이름을 가진 하위 표현식으로 분할하는 것이 더 낫다는 것을 의미합니다.

## 함수 선언

* 변수형 선언 보다 함수 구문이 좋습니다. 이렇게 하면 익명 함수를 구분하는 데 도움이 됩니다

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

* 유효성은 빨리 검사하고 값 또한 되도록 빨리 반환시켜주는 것이 좋습니다

```lua
-- 나쁜예
local function is_좋은예_name(name, options, arg)
   local is_좋은예 = #name > 3
   is_좋은예 = is_좋은예 and #name < 30

   -- ...stuff...

   return is_좋은예
end

-- 좋은예
local function is_좋은예_name(name, options, args)
   if #name < 3 or #name > 30 then
      return false
   end

   -- ...stuff...

   return true
end
```

## 함수 호출

* 함수 호출시 문자열 인수가 1개라면 괄호를 생략 가능 하지만 되도록 하지 않습니다

```lua
-- 나쁜예
local data = get_data"KRP"..tostring(area_number)
-- 좋은예
local data = get_data("KRP"..tostring(area_number))
local data = get_data("KRP")..tostring(area_number)
```

* 함수 인자로 테이블 을 1개만 받을 경우 괄호를 생략합니다

```lua
local an_instance = a_module.new {
   a_parameter = 42,
   another_parameter = "yay",
}
```

> **해석:** 위와 같이 한 개의 테이블을 인자로 받는 경우는 우선순위 문제가 발생하지 않습니다

## 테이블 속성

* 알고 있는 테이블 값에 접근 할 때에는 `.` 표기법을 사용합니다

```lua
local luke = {
   jedi = true,
   age = 28,
}

-- 나쁜예
local is_jedi = luke["jedi"]

-- 좋은예
local is_jedi = luke.jedi
```

* 변수가 있는 속성에 액세스하거나 테이블을 목록으로 사용하는 경우 대괄호 표기법 `[]` 을 사용합니다.

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

## 테이블 안의 함수

* 모듈 및 클래스를 선언할 때 테이블 외부에 함수를 선언합니다

```lua
-- 나쁜예
local my_module = {
   a = function(x)
   -- code
   end
}

-- 좋은예
local my_module = {}

function my_module.a(x)
   -- code
end
```

* 메타 테이블을 선언할 때 테이블 정의 내부에 함수를 선언합니다

```lua
local version_mt = {
   __eq = function(a, b)
      -- code
   end,
   __lt = function(a, b)
      -- code
   end,
}
```

## 변수 선언

* 변수를 선언할 때는 항상 `local` 을 사용하세요.

```lua
-- 나쁜예
superpower = get_superpower()

-- 좋은예
local superpower = get_superpower()
```

> **해석:** 전역 공간의 오염을 피하기 위한 방법입니다


## 변수 스코프

* 가능한 가장 작은 범위 단위로 변수를 할당하세요

```lua
-- 나쁜예
local function 좋은예()
   local name = get_name()

   test()
   print("doing stuff..")

   --...other stuff...

   if name == "test" then
      return false
   end

   return name
end

-- 좋은예
local 나쁜예 = function()
   test()
   print("doing stuff..")

   --...other stuff...

   local name = get_name()

   if name == "test" then
      return false
   end

   return name
end
```

> **해석:** Lua에는 적절한 렉시컬 스코프가 있습니다. 변수의 범위가 좁을 수록 이펙트를 확인하기 쉬워집니다

## 조건식

* `False` 및 `nil` 은 조건식에서 거짓입니다. `false` 와 `nil` 의 차이를 알아야 하는 경우가 아니면 아래와 같은 방법이 좋습니다


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

* `nil` 과 `false` 의 차이에 의존하는 API를 설계하지 마세요

* 삼항 연산자가 더 간단한 코드를 생성하는 경우 및/또는 관용구를 사용합니다. 표현식을 중첩할 때 괄호를 사용하여 시각적으로 쉽게 스캔할 수 있습니다.


```lua
local function default_name(name)
   -- return the default "Waldo" if name is nil
   return name or "Waldo"
end

local function brew_coffee(machine)
   return (machine and machine.is_loaded) and "coffee brewing" or "fill your water"
end
```

 * 리턴 값으로 `nil` 을 내뱉는 함수 디자인을 피해주세요

## 블록

* `then break`, `break`, `return`, `람다식` 에만 한줄 블록을 사용합니다

```lua
-- 좋은예
if test then break end

-- 좋은예
if not ok then return nil, "this failed for this reason: " .. reason end

-- 좋은예
use_callback(x, function(k) return k.last end)

-- 좋은예
if test then
  return false
end

-- 나쁜예
if test < 1 and do_complicated_function(test) == false or seven == 8 and nine == 10 then do_other_complicated_function() end

-- 좋은예
if test < 1 and do_complicated_function(test) == false or seven == 8 and nine == 10 then
   do_other_complicated_function() 
   return false 
end
```

* 문장을 여러 줄로 분리하세요. 구분자로 세미콜론을 사용하지 마세요

```lua
-- 나쁜예
local whatever = "sure";
a = 1; b = 2

-- 좋은예
local whatever = "sure"
a = 1
b = 2
```

## 주석 스페이싱

* `--` 뒤에 스페이스를 사용합니다

```lua
--나쁜예
-- 좋은예
```

* 항상 쉼표 뒤와 연산자와 기호 사이에 공백을 넣으세요

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

* 선언이나 인수에서 함수 이름 뒤에 공백이 없어야 합니다.

```lua
-- 나쁜예
local function hello ( name, language )
   -- code
end

-- 좋은예
local function hello(name, language)
   -- code
end
```

* 함수 사이에는 빈 줄을 추가해주세요

```lua
-- 나쁜예
local function foo()
   -- code
end
local function bar()
   -- code
end

-- 좋은예
local function foo()
   -- code
end

local function bar()
   -- code
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

> **해석:** `git blame` 에 노이즈를 추가시킵니다

* 논리적 대응이 강조되어야 할 때 정렬이 때때로 유용합니다.

```lua
-- okay
sys_command(form, UI_FORM_UPDATE_NODE, "a",      FORM_NODE_HIDDEN,  false)
sys_command(form, UI_FORM_UPDATE_NODE, "sample", FORM_NODE_VISIBLE, false)
```

## 타입 검사

* 성능이 중요하지 않은 코드에서는 `assert()` 함수를 통한 형식검사가 좋습니다

```lua
function manif.load_manifest(repo_url, lua_version)
   assert(type(repo_url) == "string")
   assert(type(lua_version) == "string" or not lua_version)

   -- ...
end
```

> **Rationale:** 이것은 LuaRocks 개발 초기에 채택된 방식입니다

* 형식 변환시 표준 함수를 사용하세요

```lua
-- 나쁜예
local total_score = review_score .. ""

-- 좋은예
local total_score = tostring(review_score)
```

## Errors

* 예상되는 이유로 실패할 수 있는 함수(예: I/O)는 오류 시 nil 및 (문자열) 오류 메시지를 반환해야 하며, 그 뒤에 오류 코드와 같은 다른 반환 값이 올 수 있습니다.

* API 오용과 같은 오류가 발생하면 error() 또는 assert()와 함께 오류가 발생해야 합니다.

## Modules

[모듈 작성 지침](http://hisham.hm/2014/01/02/how-to-write-lua-modules-in-a-post-module-world/)

* 항상 모듈 전체 이름의 마지막 구성 요소를 따서 명명된 로컬 변수에 모듈을 요구합니다.

```lua
local bar = require("foo.bar") -- requiring the module

bar.say("hello") -- using the module
```

* 임의로 모듈의 이름을 바꾸지 마십시오.

```lua
-- 나쁜예
local skt = require("socket")
```

> **Rationale:** 모듈을 호출하기로 선택한 방법을 확인하기 위해 맨 위로 계속 돌아가야 하는 경우 코드를 읽기가 훨씬 더 어렵습니다.

* 모듈을 요구하는 데 사용되는 동일한 전체 소문자 로컬 이름을 사용하여 테이블을 선언하여 모듈을 시작하십시오. 전체 모듈 경로를 식별하기 위해 LDoc 주석을 사용할 수 있습니다.

```lua
--- @module foo.bar
local bar = {}
```

* 지역 변수와 충돌하지 않는 이름을 사용하십시오. 예를 들어, "크기"와 같은 모듈 이름을 지정하지 마십시오.

* 로컬 함수를 사용하여 로컬 함수만 선언합니다. 즉, 모듈 외부에서 액세스할 수 없는 함수입니다.

즉, 로컬 함수 helper_foo()는 helper_foo가 실제로 로컬임을 의미합니다.

* 공용 함수는 점 구문을 사용하여 모듈 테이블에 선언됩니다.

```lua
function bar.say(greeting)
   print(greeting)
end
```

> **Rationale:** Visibility rules are made explicit through syntax. 

* 모듈에 전역을 설정하지 말고 항상 마지막에 테이블을 반환하십시오.

* 모듈을 함수 처럼 사용하려면 모듈 테이블에 __call 메타메소드를 설정 하세요

> **Rationale:** 모듈은 Lua 인터프리터 또는 기타 도구를 통해 내용을 검사할 수 있도록 테이블을 반환해야 합니다.

* 모듈을 `require` 하면 다른 모듈을 로드하고 모듈 테이블을 반환하는 것 외에는 부작용이 발생하지 않아야 합니다.

* 모듈에는 상태 값이나 변수가 없어야 합니다. 모듈에 구성이 필요한 경우 `factory` 에게 위임하세요.
예를 들어 다음과 같이 만들지 마세요

```lua
-- 나쁜예
local mp = require "MessagePack"
mp.set_integer("unsigned")
```

위 방법보단 아래가 좋습니다

```lua
-- 좋은예
local messagepack = require("messagepack")
local mpack = messagepack.new({integer = "unsigned"})
```

* require 호출은 일반적인 Lua 함수 호출처럼 보여야 합니다.

```lua
-- 나쁜예
local bla = require "bla"

-- 좋은예
local bla = require("bla")
```

> **해석:** 이것은 require가 키워드가 아니라 함수 호출이라는 것을 명시적으로 만듭니다. 다른 많은 언어는 이 목적을 위해 키워드를 사용하므로 require에 "특별한 구문"을 사용하면 Lua를 처음 접하는 사람들을 곤경에 빠뜨릴 수 있습니다.

## 객체 지향

* 다음과 같은 방법으로 클래스를 만듭니다.

```lua
--- @module myproject.myclass
local myclass = {}

-- class table
local MyClass = {}

function MyClass:some_method()
   -- code
end

function MyClass:another_one()
   self:some_method()
   -- more code
end

function myclass.new()
   local self = {}
   setmetatable(self, { __index = MyClass })
   return self
end

return myclass
```

* 클래스 테이블과 클래스 메타테이블은 모두 로컬이어야 합니다. 메타메소드를 포함하는 경우 메타테이블은 MyClass_mt라는 최상위 로컬로 선언될 수 있습니다.

> **해석:** 
위의 코드에서 서명에 MyClass가 있는 함수가 메서드라는 것을 쉽게 알 수 있습니다. 이에 대한 설계 근거에 대한 더 깊은 논의는 [여기](http://hisham.hm/2014/01/02/how-to-write-lua-modules-in-a-post-module-world/).에서 찾을 수 있습니다. 

* 메소드를 호출할 때 메소드 표기법을 사용하십시오:

```lua
-- 나쁜예 
my_object.my_method(my_object)
-- 좋은예
my_object:my_method()
```

> **해석:** 이것은 의도가 OOP 메소드로 함수를 사용하는 것임을 명시합니다.


* 메모리 이외의 리소스를 해제하기 위해 __gc 메타메서드에 의존하지 마십시오. 객체가 파일과 같은 리소스를 관리하는 경우 해당 API에 닫기 메서드를 추가하고 __gc를 통해 자동으로 닫지 마십시오. __gc를 통한 자동 닫기는 모듈 사용자가 가능한 한 빨리 리소스를 닫지 않도록 유도합니다. (표준 io 라이브러리는 이 권장 사항을 따르지 않으며 사용자는 종종 파일을 즉시 닫지 않으면 프로그램이 잠시 실행될 때 "열린 파일이 너무 많음" 오류가 발생할 수 있다는 사실을 잊어버립니다.)

> **해석:** 가비지 수집기는 메모리만 처리하는 자동 메모리 관리를 수행합니다. 가비지 수집기가 언제 호출되는지에 대한 보장은 없으며 메모리 압력은 다른 리소스에 대한 압력과 상관 관계가 없습니다.

## 파일 구조

* Lua 파일의 이름은 모두 소문자로 지정해야 합니다.

* Lua 파일은 최상위 src 디렉토리에 있어야 합니다. 메인 라이브러리 파일은 modulename.lua라고 해야 합니다.

* 테스트는 최상위 사양 디렉토리에 있어야 합니다. LuaRocks는 테스트를 위해 [Busted](http://olivinelabs.com/busted/)를 사용합니다.

* 실행 파일은 src/bin 디렉토리에 있어야 합니다.

## 정적 검사

코드가 [luacheck](https://github.com/mpeterv/luacheck)를 통과하는 것이 가장 좋습니다. 기본 설정이 없으면 합리적인 예외가 있는 .luacheckrc를 제공해야 합니다.

* 클래스 6xx의 uacheck 경고는 공백 문제를 나타내며 무시할 수 있습니다. 후행 공백을 "수정"하는 풀 요청을 보내지 마십시오.

> **Rationale:** Git은 Linux 커널 메일링 리스트에서 상속된 패치 파일 이메일 기반 워크플로 때문에 후행 공백에 대해 편집증적입니다. Git 도구를 적절하게 사용할 때 공백을 초과해도 Git의 색상으로 강조 표시되는 것을 제외하고는 차이가 없습니다(앞서 언급한 이유로). 이에 대한 Git의 현학적 태도는 수년 동안 많은 텍스트 편집기의 구문 강조 표시로 확산되었으며 이제 모든 사람들은 이유에 대해 답변할 수 없는 상태에서 후행 공백을 싫어한다고 말합니다. .


* 클래스 211, 212, 213의 luacheck 경고(사용되지 않은 변수, 인수 또는 루프 변수)는 사용되지 않는 변수가 명시적으로 추가된 경우 무시되어야 합니다. 당신이 그들 중 하나만 사용하는 경우에도 테이블에 있습니다. 또 다른 예는 API 이유로 주어진 서명을 따라야 하지만(예: 주어진 형식을 따르는 콜백) 일부 인수를 사용하지 않는 함수입니다. _ 변수를 추가하는 대신 함수가 구현하는 API가 무엇인지 인수에 설명하는 것이 좋습니다.


* if/elseif/else 블록의 시퀀스가 ​​"스위치/케이스" 스타일의 케이스 목록을 구현하고 케이스 중 하나가 "통과"를 의미하는 경우 luacheck 경고 542(분기인 경우 비어 있음)도 무시할 수 있습니다. 예를 들어:


```lua
if warning >= 600 and warning <= 699 then
   print("no whitespace warnings")
elseif warning == 542 then
   -- pass
else
   print("got a warning: "..warning)
end
```



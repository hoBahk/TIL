# SwiftLint

참고 - https://realm.github.io/SwiftLint/정책이름.html


## AutoCorrect 항목

    
### 라인 뒤에 공백이 없어야 함.  

trailing_whitespace

    

### 수직 공백

vertical_whitespace  

    

### 라인 앞 공백

leading_whitespace  

    

### 콜론(:) 뒤에는 공백을 주어야 함  

colon

    

### 세미콜론 붙이지 말아야 함  

trailing_semicolon

    

### 주석 달 때 한 칸 뛰고 작성  

comment_spacing

    

### 콤마 앞에 공백이 없어야 함  

comma

    

### 마지막 요소에 콤마를 붙이지 말아야 함  

trailing_comma    

### 사용되지 않은 클로저 파라미터가 있음  

unused_closure_parameter

    

### 중괄호 열기 전에 공백이 있어야 함  

opening_brace

    

### 중괄호와 소괄호가 같이 나올때 사이에 공백이 없어야함  

closing_brace

        

### 옵셔널을 nil로 초기화 하는 것은 중복된 것이다.  

redundant_optional_initialization

    

### else, catch는 앞의 끝나는 괄호와 같은 줄에 있어야 하며 한 칸 띄우고 사용해야 함  

statement_position

        

### if, for, guard, while 등 조건을 작성할 때 괄호로 묶지 않아야 함  

control_statement

    

### Swift Constructor는 legacy convenience functions 보다 선호됨  

legacy_constructor

        

### 끝에 아무것도 없는 줄이 없어야 함  

trailing_newline

    

### 메서드 이름과 괄호 사이에 공백을 넣지 않아야 함

no_space_in_method_call

        

### Prefer -> Void over -> ()

void_return

    

### 후행 클로저를 사용할 때 빈괄호는 작성하지 않는다.  

empty_parentheses_with_trailing_closure

    

### Array 대신 [Int]를 사용  

syntactic_sugar

        

### Prefer _ = foo() over let _ = foo()  

redundant_discardable_let

        

### 연관값을 사용하지 않을 때 인수를 생략할 수 있음  

empty_enum_arguments

        

### MARK 주석 형식을 제대로 사용해야 함  

mark



## 고쳐야 하는 항목



### available을 사용할 떄 조건

unavailable_condition



### 클로저 파라미터와 여는 괄호는 같은 줄에 있어야 함

closure_parameter_position

    

### 함수 파라미터 선언할 때 여러줄이 있는 경우 세로정렬 해야 함

vertical_parameter_alignment

    

### enum case 이름과 지정한 원시값 문자열이 같은 경우 작성하지 않는다.

redundant_string_enum_value



### 연산프로퍼티에 getter만 있는 경우 get을 작성하지 않고 사용해야 함 

implicit_getter

### 문서 주석에만 ‘/‘를 3개 이상 써야 함

orphaned_doc_comment

    

### Prefer using .allSatisfy() or .contains() over reduce(true) or reduce(false)
reduce_boolean


### 튜플의 요소는 2개까지만 사용
large_tuple



### 타입은 최대 1레벨 깊이까지 중첩되어야 하고, 함수는 최대 2레벨 깊이까지 중첩되어야 함
nesting

    

### 하나의 함수에 하나의 for, if문 등을 써야 함
cyclomatic_complexity

    

### 함수 줄길이 제한 

function_body_length



### 불필요한 break문 사용 금지 

unneeded_break_in_switch

    

### for문 안에 if문 하나면 where을 써라

for_where



### TODO, FIXMEs를 해결해야 함

todo





## warning, error 모두 있는 규칙



### 한 줄로는 warning의 수준만 설정할 수 있습니다. 

line_length: 300    # implicitly



### 배열을 사용해 warning과 error의 수준을 모두 설정할 수 있습니다. 

type_body_length:   
\- 600 # warning, implicitly  
\- 600 # error, implicitly



file_length:   
  warning: 1000    # explicitly 
  error: 1200    # explicitly



### 네이밍 규칙


### min\_length 및 max_length에 대한 warning/error를 설정할 수 있습니다.

### 규칙에 제외되는 특수한 이름도 지정할 수 있습니다.

type_name:

  min_length:  # warning and error   
    warning: 50  
    error: 50

  max_length: # warning and error   
    warning: 50  
    error: 50

  excluded: iPhone # excluded via string

  allowed_symbols: \[\"\_\"\] # these are allowed in type names



identifier_name: 

  min_length:  # warning and error  
    warning: 50  
    error: 50

  max_length: # warning and error     
    warning: 50   
    error: 50

  excluded: iPhone # excluded via string

  allowed_symbols: \[\"\_\"\] # these are allowed in type names



### SwiftLint 검사에서 제외할 파일 경로

excluded:
\- Pods


included:    
\- project
---
title: "Dependency Injection in Java"
categories:
  - Programming
  - Java
tags:
  - Dependency Injection
  - Spring Framework
  - Software Design
  last_modified_at: "{{ 'now' | date: '%Y-%m-%d %H:%M:%S %z' }}"
# last_modified_at: 2024-07-09T10:20:00-08:00
---

## Dependency Injection, DI
1. 생성자 주입 (Constructor Injection):
    - 가장 권장되는 방법입니다.
    - 불변성을 보장하고 순환 참조를 방지할 수 있습니다.   
```java   
@Controller
public class UserController {
    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```
2.필드 주입 (Field Injection):
    - 간단하지만 테스트하기 어렵고 순환 참조 문제가 발생할 수 있어 권장되지 않습니다.   
```java   
@Controller
public class UserController {
    @Autowired
    private UserService userService;
}
```
3. 세터 주입 (Setter Injection):

    - 선택적 의존성에 유용하지만, 불변성을 보장하지 않습니다.   
```java   
@Controller
public class UserController {
    private UserService userService;

    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    }
}
```
4. 메서드 주입 (Method Injection):
    - 거의 사용되지 않지만, 특수한 경우에 유용할 수 있습니다.   
```java   
@Controller
public class UserController {
    private UserService userService;

    @Autowired
    public void init(UserService userService) {
        this.userService = userService;
    }
}
```
5. 인터페이스를 통한 주입:
    - 느슨한 결합을 위해 인터페이스를 사용할 수 있습니다.
    - 여기서 UserService는 인터페이스이고, 실제 구현체는 Spring이 주입합니다.    
```java   
@Controller
public class UserController {
    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```
6. 생성자 주입 시 @Autowired 생략:
    - Spring 4.3 이후부터는 단일 생성자의 경우 @Autowired를 생략할 수 있습니다.   
```java   
@Controller
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```    
### 대부분의 경우 생성자 주입이 권장됩니다. 이는 필수적인 의존성을 명확히 하고, 테스트하기 쉬우며, 불변성을    보장하기 때문입니다. 또한 순환 참조 문제를 컴파일 시범에 발견할 수 있어 안전합니다.     
### 이러한 방법들을 상황에 맞게 선택하여 사용하면 됩니다. 의존성 주입을 통해 코드의   
    결합도를 낮추고 유지보수성과 테스트 용이성을 높일 수 있습니다.

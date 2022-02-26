- 环境背景

```
tomcat7
```

- maven依赖

```
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.1.1.Final</version>
        </dependency>
```

- 实体类上添加校验注解，例如：

```java
publicclass Person {

    @NotNull(message = "classId 不能为空")
    private String classId;

    @Size(max = 33)
    @NotNull(message = "name 不能为空")
    private String name;

    @Pattern(regexp = "((^Man$|^Woman$|^UGM$))", message = "sex 值不在可选范围")
    @NotNull(message = "sex 不能为空")
    private String sex;

    @Email(message = "email 格式不正确")
    @NotNull(message = "email 不能为空")
    private String email;

}
```

- 在需要验证的参数上加上了`@Valid`注解，如果验证失败，它将抛出`MethodArgumentNotValidException`

```java
@RestController
@RequestMapping("/api")
publicclass PersonController {

    @PostMapping("/person")
    public ResponseEntity<Person> getPerson(@RequestBody @Valid Person person) {
        return ResponseEntity.ok().body(person);
    }
}
```

- 自定义全局异常处理器可以帮助我们捕获异常，并进行一些简单的处理

```java
@ControllerAdvice(assignableTypes = {PersonController.class})
publicclass GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(
            MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach((error) -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errors);
    }
}
```

- 遇到问题

```
hibernate-validator版本过高（6.0.9.Final），导致与tomcat不匹配，报错java.lang.ClassNotFoundException: javax.el.ELManager
```

- 参考文章

[spring和hibernate整合之---java.lang.ClassNotFoundException: javax.el.ELManager 大坑](https://www.bbsmax.com/A/amd080Ljdg/)

[今天搭建一个ssm框架的项目，报了一个令我怀疑人生的错误](https://www.cnblogs.com/zhujiqian/p/11679897.html)
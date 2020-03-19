# RequestParam与RequestBody

# 1、代码及请求结果如下

```java
 // 每个请求都使用content_type为url和json分别访问
    // get方法
    // 1、PathVariable       都可以访问
    @GetMapping("/q1/{name}")
    public List<User> q1(@PathVariable("name")String name) {
        return userService.list(new QueryWrapper<User>().eq("name", name));
    }
    // 2、RequestParam      都可以访问
    @GetMapping("/q2")
    public List<User> q2(@RequestParam("name")String name) {
        return userService.list(new QueryWrapper<User>().eq("name", name));
    }
    // 3、无注解            都可以访问
    @GetMapping("/q3")
    public List<User> q3(String name) {
        return userService.list(new QueryWrapper<User>().eq("name", name));
    }
    // 4、无注解           都可以访问
    @GetMapping("/q4")
    public List<User> q4(QueryCondition condition) {
        return userService.list(new QueryWrapper<User>().eq("name", condition.getName()));
    }
    // 5、RequestBody     都不可以访问
    @GetMapping("/q5")
    public List<User> q5(@RequestBody Map<String, Object> reqMap) {
        return userService.list(new QueryWrapper<User>().eq("name", reqMap.get("name")));
    }


    // post，每个请求都使用content_type为url和json分别访问
    // 6、RequestBody  map     url不可以，json可以
    @PostMapping("/q6")
    public List<User> q6(@RequestBody Map<String, Object> reqMap) {
        return userService.list(new QueryWrapper<User>().eq("name", (String)reqMap.get("name")));
    }
    // 7、RequestBody  pojo   url不可以，json可以
    @PostMapping("/q7")
    public List<User> q7(@RequestBody QueryCondition condition) {
        return userService.list(new QueryWrapper<User>().eq("name", condition.getName()));
    }
    // 8、RequestParam map    url可以，json不可以
    @PostMapping("/q8")
    public List<User> q8(@RequestParam Map<String, Object> reqMap) {
        return userService.list(new QueryWrapper<User>().eq("name", (String)reqMap.get("name")));
    }
    // 9、RequestParam pojo    都不可以
    @PostMapping("/q9")
    public List<User> q9(@RequestParam QueryCondition condition) {
        return userService.list(new QueryWrapper<User>().eq("name", condition.getName()));
    }
    // 10、无注解，基本类型     url可以，json不可以
    @PostMapping("/q10")
    public List<User> q10(String name) {
        return userService.list(new QueryWrapper<User>().eq("name", name));
    }
    // 11、无注解 实体类       url可以，json不可以
    @PostMapping("/q11")
    public List<User> q11(QueryCondition condition) {
        return userService.list(new QueryWrapper<User>().eq("name", condition.getName()));
    }

```

# 2、总结

当为Get方法时，不用在意content_type，路径参数就用Pathvariable，？参数就用RequestParam；？参数也可以不用注解，直接用基本类型或者实体类接受。

当为Post方法时，RequestBody可以接受content_type为非url的，接受类型可以为Map或者实体类；RequestParam可以接受url的，接受类型只能为Map；在不要注解的情况下，url的可以接受，接受类型为基本类型或者实体类，而json的不可以接受。
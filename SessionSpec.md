# SessionSpec

SessoinSpec结构定义如下：

```java
public class SessionSpec{
    //service id
    private String sid;
    //username
    private String username;
    //clientId
    private String clientId
    //访问的token
    private String accessToken;
    //刷新的Token
    private String refreshToken;
    //session有效期
    private LocalDateTime expireTime;
    //session id
    private String sessionId;
}
```

Session 包含SessionSpec成员，并且提供set和get方法，以及判断session是否过期

```java
public class Session{
	
    private SessionSpec spec;
    
    public SessionSpec getSpec(){return spec;}
    
    public void setSpec(SessionSpec spec){this.spec = spec;}
    
    //judge is expired
    public boolean isExpired(){
        //当前时间已经在过期时间之后，说明已经过期
        if (LocalDateTime.now().isAfter(this.getSpec().getExpireTime())){
            return true;
        }
        return false
    }
}
```




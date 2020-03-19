####1.JWT (json web token)   

	1.是一个轻量级的规范，允许我们适用jwt在用户和服务器之间传递安全可靠的信息。  
	
####2.JWT 的组成    

参考博客：[https://www.cnblogs.com/wangshouchang/p/9551748.html](https://www.cnblogs.com/wangshouchang/p/9551748.html)

规定了token应该头部、载荷、签证 三部分构成，每个部分都是json格式

- 头部(Header)   
- 载荷(playload)    
- 签证(signature)           
   
####3.适用步骤  

1.导入依赖  

```xml 

	<dependency>
	    <groupId>io.jsonwebtoken</groupId>
	    <artifactId>jjwt</artifactId>
	    <version>0.9.1</version>
	</dependency>
```  

2.使用  

生成jwtToken  

```java   
	
	@Test
    public void generateJwtToken(){
        long tm = System.currentTimeMillis();
        String jwtToken = Jwts.builder()
                .setId(UUID.randomUUID().toString())  //jwtId,唯一id
                .setSubject("subject") //jwt主题
                .setIssuedAt(new Date(tm)) //发布时间
                .setIssuer("themelove") //发布人
                .setAudience("commonUser") //接受者
                .claim("username", "themelove") //自定义属性
                .claim("role", "admin")
                .setExpiration(new Date(tm + (1000 * 300))) //
                .setNotBefore(new Date(tm)) //该jwt不早于某个时间之前使用
                .signWith(SignatureAlgorithm.HS256, "jwtsecretkey") //签名算法，和加密key,加密key很重要
                .compact();
        System.out.println("jwtToken=>"+jwtToken);
    }

```    

解析jwtToken  

```java  

    @Test
    public void parseJwtToken(){
        String jwtToken = "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiI4MWU4NjkzOS1lNGVjLTRhZWUtOWQ4ZS1lYzk5MTAzMGIyMDAiLCJzdWIiOiJzdWJqZWN0IiwiaWF0IjoxNTg0NTM3NDAzLCJpc3MiOiJ0aGVtZWxvdmUiLCJhdWQiOiJjb21tb25Vc2VyIiwidXNlcm5hbWUiOiJ0aGVtZWxvdmUiLCJyb2xlIjoiYWRtaW4iLCJleHAiOjE1ODQ1Mzc3MDMsIm5iZiI6MTU4NDUzNzQwM30.9NxIKKM0C2VowSUniv-tYmIhUt-UHlrh9yadN46UuvY";
        Jws<Claims> jwtClaims = Jwts.parser()
                .setSigningKey("jwtsecretkey")
                .parseClaimsJws(jwtToken);
        String signature = jwtClaims.getSignature();
        Claims body = jwtClaims.getBody();
        JwsHeader header = jwtClaims.getHeader();
        System.out.println("signature="+signature);
        System.out.println("body="+body);
        System.out.println("header="+header);
    }

	#输出结果如下：
	signature=9NxIKKM0C2VowSUniv-tYmIhUt-UHlrh9yadN46UuvY
	body={jti=81e86939-e4ec-4aee-9d8e-ec991030b200, sub=subject, iat=1584537403, iss=themelove, aud=commonUser, username=themelove, role=admin, exp=1584537703, nbf=1584537403}
	header={alg=HS256}

```


- authentication & authorization
- integration with LDAP / OAuth2
- Protect against CSRF,XSS
- flexible security config

Concepts
- authentication (verify)
- authorization  (level of access)
- filters  (chain of filter that process request)
- security config

Terminology
- principal  ( user who perform action )
- authentication (confirm creds)
- authorization ( access policy) 
- Authentication (principal in Spring security manner)
- GrantedAuthority ( perm granted to principal)
- SecurityContext (hold authentication and other security stuff)
- SecurityContextHolder (access to SecurityContext)

Setup
- Namespace in web.xml
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:sec="http://www.springframework.org/schema/security"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring
-beans-3.0.xsd
http://www.springframework.org/schema/security
http://www.springframework.org/schema/security/spring-security-3.0.xsd">
```
- Option 1: setting security filter in web.xml
```xml
<filter>
	<filter-name>springSecurityFilterChain</filter-name>
	<filter-class>
	org.springframework.web.filter.DelegatingFilterProxy
	</filter-class>
</filter>

<filter-mapping>
	<filter-name>springSecurityFilterChain</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
- Option 2: SecurityWebApplicationInitializer.java
```java
package config;
import
org.springframework.security.web.context.AbstractSecurityWebApplicationInitializer;

public class SecurityWebApplicationInitializer extends
AbstractSecurityWebApplicationInitializer {
// no code required here
}
```
- dependencies in pom.xml
```xml
 <dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-web</artifactId>
	<version>5.3.13.RELEASE</version>
</dependency>

<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-config</artifactId>
	<version>5.3.13.RELEASE</version>
</dependency>
```
- Security Config

In memory Authentication
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extend WebSecurityConfigAdapter throws Exception{


@Override
protected void configure(AuthenticationManagerBuilder auth){
	auth.inMemoryAuthentication()
		.withUser("admin").password("{noop}password")
		.roles("ADMIN")
		.and()
		.withUser("student")
		.password(passwordEncoder.encode("password"))
		.roles("STUDENT");
}



public BCryptPasswordEncoder passwordEncoder(){
	return new BCryptPasswordEncoder();
}


```


Database Authentication
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extend WebSecurityConfigAdapter throws Exception{

@AutoWired
DataSource dataSrc;

@Override
protected void configure(AuthenticationManagerBuilder auth){
	auth.jdbcAuthentication()
		.dataSource(dataSrc)
		.passwordEncoder(getPasswordEncoder())
		.usersByUsernameQuery("SELECT username, password, enabled FROM users WHERE username = ?")
        .authoritiesByUsernameQuery("SELECT username, authority FROM authorities WHERE username");
}



public BCryptPasswordEncoder getPasswordEncoder(){
	return new BCryptPasswordEncoder();
}
```


LDAP Authentication
```java
Not relevant
```



### Authorization

- can alternatively use @PreAuthorize("hasRole(''admin)") in Controllers

| methods                      | desc                                       |
| ---------------------------- | ------------------------------------------ |
| authorizeRequest()           | access rules for specific end points       |
| antMatchers()                | url pattern to match                       |
| formLogin()                  | enable login form                          |
|                              |                                            |
|                              |                                            |
| hasRole()                    | access based on role                       |
| hasAnyRole()                 | access based on role                       |
| authenticated()              | requires authentication , but any role can |
| anonymous()                  | no authentication only                     |
| permitAll()                  | Allow access to all,, w/o authentication   |
| denyAll()                    |                                            |
|                              |                                            |
| anyRequest().authenticated() | any request need authentication            |
|                              |                                            |
| failureUrl()                 | redirect after fail authenticate (login)   |
| defaultSuccessUrl()          | redirect after success login               |
| logoutSuccessUrl()           | redirect after success logout              |



```java

@AutoWired
CustomAuthenticationSuccessHandler CUSTOM;

public void configure(HttpSecurity http){
	http
	.authorizeRequest()
	.antMatchers("/secret").hasRole("Admin")
	
	.antMatchers("/admin/**").hasRole("Admin")
	.antMatchers("/student/**").hasRole("Student")
	.antMatchers("/teacher/**").hasRole("Teacher")
	
	.antMatchers("/report").hasAnyRole("Teacher","Student")
	.antMatchers("/teacher/**").hasRole("Teacher")

	.antMatchers("/free").authenticated()
	.antMatchers("/noway").anonymous()
	
	.anyRequest().authenticated()
	
	.and()
	.formLogin()
		.loginPage("/url")
		.failureUrl("/url?error")
		.successHandler(CUSTOM)
		.defaultSuccessUrl("/welcomepage", true)
		.permitAll()

	.and()
	.logout()
		.logoutSuccessUrl("/byePage", true)
		.permitAll();
	.and()
	.exceptionHandling()
		.accessDeniedPage("/customPAGE403");
		
}




@Component

public class CustomAuthenticationSuccessHandler implements AuthenticationSuccessHandler {
  
    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {
        String role = authentication.getAuthorities().stream()
                .map(grantedAuthority -> grantedAuthority.getAuthority())
                .findFirst()
                .orElse("");
  

        String redirectUrl = "/";
        if (role.equals("ROLE_ADMIN")) {
            redirectUrl = "/SpringLabApp3/users/admin";
        } else if (role.equals("ROLE_TEACHER")) {
            redirectUrl = "/SpringLabApp3/users/teacher";
        } else if (role.equals("ROLE_STUDENT")) {
            redirectUrl = "/SpringLabApp3/users/student";
        }

  
        response.sendRedirect(redirectUrl);
    }
}
```



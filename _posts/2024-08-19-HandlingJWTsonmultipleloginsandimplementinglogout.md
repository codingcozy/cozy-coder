---
title: "여러 로그인 상황에서 JWT 다루기와 로그아웃 구현 방법"
description: ""
coverImage: "/assets/img/2024-08-19-HandlingJWTsonmultipleloginsandimplementinglogout_0.png"
date: 2024-08-19 02:36
ogImage: 
  url: /assets/img/2024-08-19-HandlingJWTsonmultipleloginsandimplementinglogout_0.png
tag: Tech
originalTitle: "Handling JWTs on multiple logins and implementing logout"
link: "https://medium.com/@shashwat2002joshi/handling-jwts-on-multiple-logins-and-implementing-logout-1d7348620e8a"
isUpdated: true
updatedAt: 1724033293430
---


# 우리가 논의할 것들:

- 여러 로그인 시 이전 토큰을 무효화하는 방법.
- JWT를 사용할 때 개별 장치/모든 장치에서 로그아웃을 처리하는 방법, 관련된 토큰을 무효화하는 방법.

# 조금의 배경 정보:

먼저 JWT가 stateless임을 이해해야 합니다. 한 번 생성되고 사용자에게 전송된 후에는 서버가 제어할 수 없습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

멀티 로그인시나 사용자가 로그아웃할 때 JWT를 강제로 무효화할 수는 없습니다. 이 JWT들은 만료 시간에 도달할 때만 무효화될 수 있습니다.

그러나 이에는 많은 문제가 있습니다. 사용자에 대해 여러 유효한 JWT를 발급하고 이를 강제로 만료시킬 방법이 없습니다. 유효한 토큰이 많아질수록 위험이 커집니다.


![HandlingJWTsonmultipleloginsandimplementinglogout](/assets/img/2024-08-19-HandlingJWTsonmultipleloginsandimplementinglogout_0.png)


# 해결책 🤷‍♂️

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그래서 이 문제를 해결하고 로그아웃 기능을 구현하기 위해서는 어떻게든 서버 측에서 이 토큰들의 상태를 유지해야 합니다.

토큰을 지속시키는 가장 좋은 방법은 무엇인가요?

맞추셨네요. 사용자 토큰을 저장할 테이블을 생성하는 거죠.

무서우세요? 네, 확실히 그렇습니다. 모든 토큰을 한곳에 저장하는 것은 확실히 좋지 않은 아이디어입니다. 하지만 로그아웃을 구현하고 여러 로그인에서 이전 토큰을 무효화하려면 다른 선택이 없습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

저희 서비스를 안전하게 보호하기 위해서 제3자 보안 금고를 사용하여 이러한 토큰들을 저장하고, 최상의 보안 관행을 적용하고 그 외 다양한 조치를 취할 수 있습니다.

# 구현을 시작해봅시다.

먼저, 토큰 테이블을 생성해야 합니다. 이 테이블에는 토큰들과 몇 가지 다른 필드들을 저장할 것입니다:

![tokens table](/assets/img/2024-08-19-HandlingJWTsonmultipleloginsandimplementinglogout_1.png)

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이 토큰 테이블을 다루어야 할 장소는 다음과 같습니다:

- 사용자 로그인 또는 가입(이전 토큰 무효화 및 새 항목 생성)
- 필터 수준에서 JWT 유효성 확인하는 동안
- 사용자 로그아웃

하나씩 살펴봅시다.

Token.java

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```java
package com.auth.jwtauthentication.entity;

import com.auth.jwtauthentication.domain.types.TokenType;
import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "token")
public class Token {

    @Id
    @GeneratedValue // Default value is auto
    private Integer id;

    private String token;

    @Enumerated(EnumType.STRING)
    private TokenType tokenType;

    private boolean isRevoked;

    private boolean isExpired;

    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
}
```

TokenRepository.java

```java
package com.auth.jwtauthentication.repository;

import com.auth.jwtauthentication.entity.Token;
import jakarta.transaction.Transactional;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;

import java.util.Optional;

@Transactional
public interface TokenRepository extends JpaRepository<Token, Integer> {

    Optional<Token> findByToken(String token);

    @Modifying
    @Query("update Token set isRevoked = true where isRevoked = false and user.id = :userId")
    void revokeOldTokens(Integer userId);
}
```

# 사용자 로그인 또는 회원 가입


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```java
package com.auth.jwtauthentication.service;

import ...

@Service
public class AuthService {

    private final UserRepository userRepository;
    private final TokenRepository tokenRepository;
    private final PasswordEncoder passwordEncoder;
    private final JwtService jwtService;
    private final AuthenticationManager authenticationManager;

    @Autowired
    public AuthService(
            UserRepository userRepository,
            TokenRepository tokenRepository,
            PasswordEncoder passwordEncoder,
            JwtService jwtService,
            AuthenticationManager authenticationManager) {
        this.userRepository = userRepository;
        this.tokenRepository = tokenRepository;
        this.passwordEncoder = passwordEncoder;
        this.jwtService = jwtService;
        this.authenticationManager = authenticationManager;
    }

    public AuthenticationResponse register(RegisterRequest request) {
        final User USER = User.builder()
                .firstName(request.getFirstName())
                .lastName(request.getLastName())
                .email(request.getEmail())
                .password(passwordEncoder.encode(request.getPassword()))
                .role(Role.USER)
                .build();

        final User SAVED_USER = userRepository.save(USER);
        final String JWT_TOKEN = jwtService.generateToken(SAVED_USER);

        final String SAVED_JWT_TOKEN = saveNewToken(SAVED_USER, JWT_TOKEN);

        return AuthenticationResponse.builder()
                .token(SAVED_JWT_TOKEN)
                .build();
    }

    public AuthenticationResponse authenticate(AuthenticationRequest request) {
        authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                        request.getEmail(),
                        request.getPassword()
                )
        );

        final User SAVED_USER = userRepository.findByEmail(request.getEmail())
                .orElseThrow();
        final String JWT_TOKEN = jwtService.generateToken(SAVED_USER);

        final String SAVED_JWT_TOKEN = saveNewToken(SAVED_USER, JWT_TOKEN);

        return AuthenticationResponse.builder()
                .token(SAVED_JWT_TOKEN)
                .build();
    }

    private String saveNewToken(final User SAVED_USER, final String JWT_TOKEN) {
        Token token = Token.builder()
                .token(JWT_TOKEN)
                .user(SAVED_USER)
                .tokenType(TokenType.BEARER)
                .isRevoked(false)
                .isExpired(false)
                .build();

        // Invalidate old tokens
        tokenRepository.revokeOldTokens(SAVED_USER.getId());

        // Save new token
        final Token SAVED_TOKEN = tokenRepository.save(token);
        return SAVED_TOKEN.getToken();
    }
}
```

# JWT 유효성을 필터 수준에서 확인하는 동안

```java
public boolean isTokenValid(final String TOKEN, final UserDetails USER) {
    Optional<Token> SAVED_TOKEN = tokenRepository.findByToken(TOKEN);
    if(SAVED_TOKEN.isEmpty()) return false;

    final String TOKEN_FROM_DB = SAVED_TOKEN.get().getToken();

    // 사용자명 검증
    final String USERNAME = extractUsername(TOKEN_FROM_DB);
    if(!USERNAME.equals(USER.getUsername())) return false;

    // 토큰 만료 검증
    final Date CURRENT_TIME = new Date(System.currentTimeMillis());
    final Date EXPIRY_TIME = extractClaims(TOKEN_FROM_DB, Claims::getExpiration);
    if(EXPIRY_TIME.before(CURRENT_TIME)) {
        // DB 업데이트 후 false 반환
        SAVED_TOKEN.get().setExpired(true);
        tokenRepository.save(SAVED_TOKEN.get());
        return false;
    }

    // 폐기된 또는 만료된 토큰 확인
    if(SAVED_TOKEN.get().isExpired() || SAVED_TOKEN.get().isRevoked()) return false;

    return true;
}
```

# 로그아웃 핸들러


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```java
package com.auth.jwtauthentication.service;

import com.auth.jwtauthentication.entity.Token;
import com.auth.jwtauthentication.repository.TokenRepository;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.logout.LogoutHandler;
import org.springframework.stereotype.Service;

import java.util.Optional;

@Service
public class LogoutService implements LogoutHandler {

    private final TokenRepository tokenRepository;

    @Autowired
    public LogoutService(TokenRepository tokenRepository) {
        this.tokenRepository = tokenRepository;
    }

    @Override
    public void logout(
            HttpServletRequest request,
            HttpServletResponse response,
            Authentication authentication
    ) {
        final String AUTH_HEADER = request.getHeader("Authorization");
        final String USER_SENT_JWT, USER_EMAIL;

        if (null == AUTH_HEADER || !AUTH_HEADER.startsWith("Bearer ")) {
            return;
        }

        USER_SENT_JWT = AUTH_HEADER.substring(7);

        Optional<Token> SAVED_TOKEN = tokenRepository.findByToken(USER_SENT_JWT);
        if (SAVED_TOKEN.isPresent()) {
            SAVED_TOKEN.get().setExpired(true);
            SAVED_TOKEN.get().setRevoked(true);
            tokenRepository.save(SAVED_TOKEN.get());
        }
    }
}
```

전체 소스는 다음 주소에서 확인할 수 있습니다 — https://github.com/Shashwat-Joshi/Project-Gotham/tree/main/1-Authentication/1.3-JWTAuthentication

# 추가 정보 ✨

장치 수준에서 로그아웃을 수행하려면 사용자가 사용하는 장치를 나타내는 고유한 ID를 저장해야 합니다. mac 주소나 장치마다 달라지는 다른 고유한 값일 수 있습니다.


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그 경우, 로그아웃을 할 때 이 특정 기기의 모든 토큰을 무효화하도록 지정할 수 있습니다.

독자 여러분, 읽어 주셔서 감사합니다 ❤️ 궁금한 점이 있으면 언제든지 연락해 주세요. 저의 연락처는 아래에 남겨 두겠습니다.

LinkedIN: https://www.linkedin.com/in/shashwatjoshi23/
메일: shashwat2002joshi@gmail.com
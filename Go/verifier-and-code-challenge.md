# Verifier & Code Challenge (PHP→GO)

 I’m writing this because I found it really challenging, at least for me.

> In order for the server to correlate the authorization and user access code requests, a unique `code_verifier` needs to be generated for each request. The hashed value of this code (the `code_challenge`) is sent with the authorization code request, and the unhashed value (the `code_verifier`) is sent with the user access token request.
>
>
> This `code_verifier`, `code_challenge` pair is needed to confirm to the SSO server that the token request is coming from the same party as the authorization request, preventing several classes of **vulnerabilities**.
>
> The `code_verifier` is a cryptographically secure, random string between 43 and 128 characters long. The `code_verifier` comprises the characters `A-Z`, `a-z`, `0-9`, and the punctuation characters `-._~` (hyphen, period, underscore, and tilde).

To ensure the random codes we generate MUST get the ability to be read and parseable through the URL address, we have to trim several characters such as `=,+,/`.

This is how it’s being done in PHP.

```php
public static function base64url_encode($plainText): string
{
    $base64    = base64_encode($plainText);
    $base64    = trim($base64, "=");
    $base64url = strtr($base64, '+/', '-_');
    return ($base64url);
}

public static function generateRandomPseudo()
{
    return bin2hex(openssl_random_pseudo_bytes(32));
}

/*
 * Generate Verifier and Challenge
 * @return string[]
 */
public static function generateVerifierAndChallenge()
{
    $random    = self::generateRandomPseudo();
    $verifier  = self::base64url_encode(pack('H*', $random));
    $challenge = self::base64url_encode(pack('H*', hash('sha256', $verifier)));
    return [$verifier, $challenge];
}
```

To use the Go language,

```go
func GenerateVerifierAndChallenge() (*string, *string, error) {
    token := make([]byte, 32)
    _, err := rand.Read(token)
    if err != nil {
        return nil, nil, err
    }

    verifier := base64.RawURLEncoding.EncodeToString([]byte(hex.EncodeToString(token)))
    hash := sha256.New()
    hash.Write([]byte(verifier))
    hashed := hash.Sum(nil)
    sha256.Sum256([]byte(verifier))
    challange := base64.RawURLEncoding.EncodeToString(hashed)

    return &verifier, &challange, nil
}
```

# XSRF (Cross Site Request Forgery)- DVWA Walkhtrough

- autor: [Mateusz Głuchowski](https://github.com/Hue1337)

## Wprowadzenie:
- Atak `XSRF`, nazywany również `CSRF` polega na wykorzystaniu sesji oraz uprawnień użytkownika do wprowadzenia zmian na stronie/ w aplikacji.

- W tym walkthrough przejdziemy 3 poziomy trudności na maszynie podatnej na ten atak: DVWA (Damn Vulnerable Web Application).

## XSRF- Low Security
1. Zaczniemy od kilku bardzo istotnych rzeczy.
    - Upewniamy się, że jesteśmy zalogowani:
        <img src="pics/XSRF2.png"/>

    - Jeżeli nie jesteśmy, wciskamy przycisk `Logout` i logujemy się za pomocą:
        - Login: `admin`
        - Password: `password`

        <img src="pics/XSRF3.png">

    - Jeszcze zmienimy poziom trudności na łatwy:
        <img src="pics/LFI4.png"/>

    - Na koniec przechodzimy do zakładki `CSRF`:
        <img src="pics/XSRF4.png"/>

3. Następnie zobaczmy na czym stoimy. 

    - Strona umożliwia nam zmiane hasła dla obecnie zalogowanego użytkownika. W tym przypadku odpalamy `Burp Suite` i przechwytujemy requesta ze zmiany hasła:

        <img src="pics/XSRF5.png"/>

        Wykorzystamy zapytanie `GET` jakie zostaje wysłane do `backendu` aplikacji:

        ```
        http://192.168.X.X/DVWA/vulnerabilities/csrf/?password_new=haslo&password_conf=haslo&Change=Change
        ```

        `Url` zawiera parametry `password_new` oraz `password_conf`, pod których wartości podstawiamy nowe hasło. Przykładowo `haslo`.

        A następnie z naszego **PAYLOADA**. Ustawiamy link w przekierowaniu na powyższy:

        <img src="pics/XSRF6.png"/>

        Odpalamy lokalny serwer w folderze z pobranym **PAYLOADEM**:

        ```py
        python3 -m http.server
        ```

        I przechodzimy w przeglądarce do `localhosta`:
        
        ```
        http://127.0.0.1:8000/paylaod1.html
        ```

        <img src="pics/XSRF7.png"/>

        Korzystamy z *instrukcji* i mamy zmienione hasło:
        
        <img src="pics/XSRF8.png"/>

        Weryfikujemy nowe hasło (na nowo zmienione) i nazwe użytkownika:

        <img src="pics/XSRF9.png"/>

        Tym oto sposobem zdobywamy **flage**.

## XSRF- Medium Security

- Zaczynamy od zmiany poziomu trudności na `Medium`. Następnie powtarzamy kroki z poprzedniego poziomu trudności. Po przechwyceniu `requesta` widzimy takie informacje:

    <img src="pics/XSRF10.png"/>

    Szczególnie interesuje nas `linia 6`:
    
        ```
        Referer: http://192.168.1.44/DVWA/vulnerabilities/csrf/
        ```
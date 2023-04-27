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

        Korzystamy z instrukcji i mamy zmienione hasło.
        

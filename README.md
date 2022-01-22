# Sprawozdanie

**1. Należy dla dowolnego pliku Dockerfile (można użyć przykładowy plik z lab.11) wykorzystać GitHub Actions do zdefiniowania procesu budowania obrazu ale z wykorzystaniem “nowego silnika” czyli BuildKit. W efekcie, na DockerHub powinien się pojawić obraz zbudowany w oparciu o wybrany Dockerfile.**

Do wykonania zadania wykorzystano plik frontend.yml.
W kroku ‘Login to DockerHub’ ustawione są odwołania do secrets, w których wcześniej dodano nazwę użytkownika oraz token wygenerowany na koncie dockerhub.
W kroku ‘Build and push’ ustawiono budowę obrazu z pliku dockerfile.v1 (wykorzystano plik z lab11), oraz ustawiono tag do repozytorium dockerhub na którym ma się znaleźć zbudowany obraz.
Cały projekt dodano do repozytorium github: https://github.com/Mmaoo/PFS_Zad2 za pomocą narzędzia git:
- git init
- git branch -m main – zmiana brancha aby był zgodny z branchem w pliku frontend.yml
- git add .
- it commit -m "github-action-pass"
- git remote add PFS_Zad2 git@github.com:Mmaoo/PFS_Zad2.git
- git push -u PFS_Zad2 main

utworzony github action: https://github.com/Mmaoo/PFS_Zad2/actions/runs/1732755790

zbudowany obraz: mmaoo/lab:zad2


**2. Należy wykonać analogiczne działania jak w punkcie nr 1 ale z tą różnicą, że zbudowane będą trzy obrazy, dla trzech różnych, dowolnie wybranych, architektur sprzętowych.**

Utworzono nowy plik frontend2.yml na podstawnie poprzedniego polecenia dodając nowy krok ‘Set up QEMU’ w którym wykorzystywane jest qemu. W Kroku ‘Build and push’ z kolei dodano platformy, na które obraz ma być zbudowany

utworzony github action: https://github.com/Mmaoo/PFS_Zad2/actions/runs/1732872909

zbudowany obraz: mmaoo/lab:zad2.2


**3. Przygotowując obrazy “produkcyjne” bardzo często wykorzystywana jest metoda wieloetapowego budowania obrazów. Proszę odpowiedzieć na pytanie czy ta metoda budowania obrazów może być wykorzystana w połączeniu z GitHub Actions. Jeśli nie to proszę odpowiedź uzasadnić, np. powołaniem się na źródło informacji lub wynikiem własnych testów. Jeśli tak, to proszę wykonać czynności, analogicznie jak w punkcie nr 1, wykorzystując dowolny plik Dockerfile albo przykładowy Dockerfile wykorzystujący wieloetapowość, jaki był używany na jednym z poprzednich laboratoriów.**

Metoda automatycznego budowania obrazów może być wykorzystywana w połączeniu GitHub Actions. Do tego celu wystarczy utworzyć nowy plik Dockerfile wykorzystujący budowanie wieloetapowe (Dockerfile.v2) i ustawić go do budowania obrazu w pliku frontend3.yml w kroku ‘Build and push’

utworzony github action: https://github.com/Mmaoo/PFS_Zad2/actions/runs/1732941192

zbudowany obraz: mmaoo/lab:zad2.3

**4. Budując wielokrotnie te same obrazy w środowisku Docker, wykorzystywana jest pamięć (cache) z poprzednich “przebiegów” procesu budowania obrazów. BuildKit oferuje dodatkową funkcjonalność w zakresie możliwości przesyłania i pobierania danych cache z zewnętrznych repozytoriów. Proszę odpowiedzieć na pytanie czy GitHub Actions daje możliwość korzystania z tej funkcjonalności podczas kolejnych realizacji budowania danego obrazu. Jeśli nie to proszę odpowiedź uzasadnić, np. powołaniem się na źródło informacji. Jeśli tak to proszę wykonać czynności, analogicznie jak w punkcie nr 1 ale z wykorzystaniem operacji na cache procesu budowania.**

GitHub Actions daje możliwość korzystania z cache przy budowaniu obrazów docker. Można to uzyskać poprzez wykorzystanie jednego ze sposobów przedstawionych w dokumentacji: https://github.com/docker/build-push-action/blob/master/docs/advanced/cache.md. Wykorzystano tutaj sposób ‘Registry cache’, który pozwala na przechowywanie pamięci podręcznej w osobnym obiekcie przechowywanym w repozytorium dockerhub. 
Do pliku frontend4.yml w kroku ‘Build and push’ dodano opcje cache-from i cache-to ustawiając typ eksportera pamięci cache na ‘registry’, tryb eksportowania na ‘max’ aby wyeksportować wszystkie pośrednie warstwy budowanego obrazu, oraz dodano repozytorium dockerhub na którym cache ma się znaleźć: mmaoo/lab:zad2.4cache. Dodatkowo zmieniono branch tak aby nie były ponownie budowane obrazy z poprzednich zadań.

utworzony github action: https://github.com/Mmaoo/PFS_Zad2/actions/runs/1733071640

zbudowany obraz: mmaoo/lab:zad2.4


Poprawność działania sprawdzono podczas próby drugiego budowania obrazu:
github action dla testu: https://github.com/Mmaoo/PFS_Zad2/actions/runs/1733080108

Widać że akcja wykonała się o wiele szybciej – 41s – podczas gdy poprzednie wykonanie akcji zajęło 1m 38s. Dodatkowo po rozwinięciu wykonanych działań można zobaczyć, że obraz jest budowany na podstawie pamięci podręcznej.

![image](https://user-images.githubusercontent.com/32769933/150645226-ac4397d2-c8d1-4953-959f-84bc789d8bf8.png)

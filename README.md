# Clouds_lab_10 ☁️

## Temat: Tworzenie i wykorzystanie sieci mostkowych definiowanych przez użytkownika. 

## W pierwszym punkcie utworzyłam sieć mostkową o nazwie lab10net

☁️ `docker network create --driver=bridge --subnet=10.0.10.0/24 lab10net` 

<img width="1038" alt="Zrzut ekranu 2024-06-2 o 21 10 39" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/515d2968-5aea-4455-98c3-fdfb04d47577">

Polecenie to tworzy nową sieć mostkową w Dockerze z nazwą lab10net, używając podsieci 10.0.10.0/24. Sieć mostkowa pozwala na komunikację między kontenerami działającymi na tej samej maszynie hosta, umożliwiając im wymianę danych i współpracę w ramach jednej logicznej sieci.

## Utworzenie katalogów dla logów

☁️ `mkdir -p lab10/web1_logs`

☁️ `mkdir -p lab10/web2_logs`

☁️ `mkdir -p lab10/web3_logs` 

<img width="1042" alt="Zrzut ekranu 2024-06-2 o 21 30 10" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/efb4a7c4-0576-4ab3-ad54-1bcaedd239d7">

Polecenia te tworzą trzy katalogi (web1_logs, web2_logs, web3_logs) w katalogu lab10, tworząc również katalog lab10 jeśli nie istnieje.
Ta struktura katalogów jest tutaj używana do do przechowywania logów.

## Uruchomienie pierwszego kontenera z wolumenem
☁️ `docker run -dt --rm --name web1 --network lab10net --ip 10.0.10.10 -p 8081:80 -v $(pwd)/lab10/web1_logs:/var/log/nginx -v $(pwd)/index.html:/usr/share/nginx/html/index.html:ro nginx:latest`

<img width="1249" alt="Zrzut ekranu 2024-06-2 o 21 33 09" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/29ce0111-2849-4f6e-901a-3f0446ad7a07"> <br>

☁️ `docker run:` Polecenie służące do uruchomienia nowego kontenera Docker.

☁️`-dt`: Opcje, które oznaczają "detached" uruchomienie kontenera w tle i przydzielenie pseudo-TTY.

☁️ `--rm`: Automatycznie usuwa kontener po zakończeniu jego działania.

☁️ `--name web1`: Nadaje nazwę "web1" dla uruchamianego kontenera.

☁️ `--network lab10net`: Dołącza kontener do sieci o nazwie "lab10net".

☁️ `--ip 10.0.10.10`: Przypisuje kontenerowi adres IP "10.0.10.10" w sieci "lab10net".

☁️ `-p 8081:80`: Mapuje port 8081 hosta na port 80 w kontenerze.

☁️ `-v $(pwd)/lab10/web1_logs:/var/log/nginx`: Montuje katalog web1_logs z bieżącego katalogu roboczego hosta jako /var/log/nginx w kontenerze. Pozwala to na zapisywanie logów NGINX bezpośrednio na hoście.

☁️ `-v $(pwd)/index.html:/usr/share/nginx/html/index.html:ro`: Montuje plik index.html z bieżącego katalogu roboczego hosta jako /usr/share/nginx/html/index.html w kontenerze. Opcja `ro` oznacza, że plik jest montowany tylko do odczytu.

☁️ `nginx:latest`: Obraz Docker, który ma być używany do uruchomienia kontenera. W tym przypadku jest to najnowsza wersja obrazu NGINX.

<img width="1248" alt="Zrzut ekranu 2024-06-2 o 21 36 59" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/de363add-3d0d-4b5f-aabc-bb87bf132662"> <br>

## Uruchomienie drugiego kontenera z wolumenem sklonowanym z pierwszego kontenera

☁️ `docker run -dt --rm --name web2 --network lab10net --ip 10.0.10.11 -p 8082:80 --volumes-from web1 -v $(pwd)/lab10/web2_logs:/var/log/nginx nginx:latest`

<img width="1258" alt="Zrzut ekranu 2024-06-2 o 21 49 22" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/ad8cb5b2-7991-44ae-a45f-3b1de156f026"> <br>

☁️ `docker run`: Polecenie służące do uruchomienia nowego kontenera Docker.

☁️ `-dt`: Opcje, które oznaczają "detached" uruchomienie kontenera w tle i przydzielenie pseudo-TTY.

☁️ `--rm`: Automatycznie usuwa kontener po zakończeniu jego działania.

☁️ `--name web2`: Nadaje nazwę "web2" dla uruchamianego kontenera.

☁️ `--network lab10net`: Dołącza kontener do sieci o nazwie "lab10net".

☁️ `--ip 10.0.10.11`: Przypisuje kontenerowi adres IP "10.0.10.11" w sieci "lab10net".

☁️ `-p 8082:80`: Mapuje port 8082 hosta na port 80 w kontenerze.

☁️ `--volumes-from web1`: Kopiuje wszystkie wolumeny z kontenera o nazwie "web1". To oznacza, że wszystkie dane zapisane na wolumenach w kontenerze "web1" będą dostępne w kontenerze "web2".

☁️ `-v $(pwd)/lab10/web2_logs:/var/log/nginx`: Montuje katalog web2_logs z bieżącego katalogu roboczego hosta jako /var/log/nginx w kontenerze. Pozwala to na zapisywanie logów NGINX bezpośrednio na hoście.

☁️ `nginx:latest`: Obraz Docker, który ma być używany do uruchomienia kontenera. W tym przypadku jest to najnowsza wersja obrazu NGINX.

## Uruchomienie trzeciego kontenera z wolumenem sklonowanym z pierwszego kontenera

☁️ `docker run -dt --rm --name web3 --network lab10net --ip 10.0.10.12 -p 8083:80 --volumes-from web1 -v $(pwd)/lab10/web3_logs:/var/log/nginx nginx:latest`

<img width="1255" alt="Zrzut ekranu 2024-06-2 o 21 55 50" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/432d937c-e053-40f8-8577-b1e63879d725"> <br>

Kod uruchamia kontener NGINX, który korzysta z tych samych wolumenów co kontener "web1" i zapisuje logi do katalogu web3_logs na hoście. Kontener jest uruchamiany w sieci "lab10net" z adresem IP "10.0.10.12" i jest dostępny na hoście na porcie 8083.

## Curl 

W tym punkcie sprawdzam czy aplikacja działa zgodnie z przewidywaniami.

☁️ `curl localhost:8081`

☁️ `curl localhost:8082`

☁️ `curl localhost:8083`

<img width="1048" alt="Zrzut ekranu 2024-06-2 o 22 14 52" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/3f72f46b-57ac-466e-86ed-e0c8e7e89aec"> <br>

## Sprawdzenie logów

W tym punkcie sprawdzam zawartość logów kontenerów.

☁️ `cat lab10/web1_logs/access.log`

☁️ `cat lab10/web2_logs/access.log`

☁️ `cat lab10/web3_logs/access.log`

<img width="1039" alt="Zrzut ekranu 2024-06-2 o 22 16 47" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/197ce8d7-dbaf-48df-a546-6c6aa4a77364"> <br>


## Sprawdzenie read-only (RW musi być false dla index.html)

☁️ `docker inspect web1 | jq ".[].Mounts"`

☁️ `docker inspect web2 | jq ".[].Mounts"`

☁️ `docker inspect web3 | jq ".[].Mounts"`

- Pliki logów ustawione są na RW: true, ponieważ chcę mieć możliwość ich modyfikacji.
- Plik index.html ustawiony jest na RW: false, zgodnie z poleceniem zadania.
  
<img width="902" alt="Zrzut ekranu 2024-06-2 o 22 21 26" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/25ab24fc-fee8-4940-a6e0-164c758aff3a"><br>
<img width="918" alt="Zrzut ekranu 2024-06-2 o 22 22 19" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/9b99ab92-2492-4125-851c-aefa3193a96d"> <br>

## Sprawdzenie przyłączenia kontenerów

☁️ `docker inspect network lab10net | jq '.[].Containers'`

To polecenie wyświetla informacje o wszystkich kontenerach w sieci Docker o nazwie lab10net.

<img width="901" alt="Zrzut ekranu 2024-06-2 o 22 23 18" src="https://github.com/radzp/Clouds_lab_10/assets/98003017/d284b5a9-5db0-41f9-a95e-5c6bf74a2435">



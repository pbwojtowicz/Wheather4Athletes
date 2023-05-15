## Projekt aplikacji zaliczeniowej SPA - aplikacja określająca warunki do uprawiania sportu na podstawie danych meteorologicznych i jakości powietrza

Projekt przedstawia aplikację, która umożliwia sprawdzenie warunków meteorologicznych i zanieczyszczenia powietrza dla konkretnych dyscyplin sportowych. Aplikacja pozwala na wyszukanie miejscowości po wpisaniu jej nazwy w odpowiednim polu (dodatkowo ją podopowiada) lub znalezienie za pomocą geolokacji. Dodatkowo użytkownik ma możliwość dodania lokalizacji do listy ulubionych, które przechowywane są w pamięci lokalnej - ten zabieg spowodował, że dane są zapisane nawet po zamknięciu przeglądarki. 

W oknach aplikacji dostępne są informacje o aktualnych i prognozowanych warunkach - zarówno pogodowych oraz jakości powietrza. Przekazywana jest także sugestia dotycząca uprawiania danej dyscypliny sportowej na zewnątrz spośród predefiniowanych: bieganie, rower, street workout. Warunki do ich uprawiania są zależne od szeregu czynników (temperatura, siła wiatru, opady deszczu, zanieczyszczenie powietrza.

<b><a href="http://wizard.uek.krakow.pl/~s200821/">LINK DO URUCHOMIENIA APLIKACJI</a></b>

### Funkcjonalności

Aplikacja umożliwia:
- wyszukiwanie miejscowości za pomocą:
  - wyszukiwarki (z podpowiedziami)
  - gelokacji
- dodawanie miejsc do ulubionych
- zapisane miejsca są przechowywane w pamięci lokalnej
- wyświetlanie aktualnej pogody i opisu warunków atmosferycznych
- wyświetlanie aktualnego pomiaru zanieczyszczenia (razem z lokalizacją punktu pomiarowego), określanie procentowej, dobowej normy wg. WHO
- wyświetlanie wykresów z przewidywanymi (na przestrzeni 21h):
  - temperaturami 
  - opadami i prędkościami wiatru
  - zanieczyszczeniami powietrza (PM10 oraz PM2.5)
- wyświetlenie tekstowej sugestii dot. uprawiania sportu w danym momencie (wraz z wyborem danej dyscypliny)


### Zrzuty ekranu

<img src="http://wizard.uek.krakow.pl/~s200821/rest/zdj_spa/1.png">
<img src="http://wizard.uek.krakow.pl/~s200821/rest/zdj_spa/3.png">
<img src="http://wizard.uek.krakow.pl/~s200821/rest/zdj_spa/2.png">
<img src="http://wizard.uek.krakow.pl/~s200821/rest/zdj_spa/5.png">

### Wykorzystane zasoby
<ul>
<li><a href="https://airly.eu/">Airly API</a></li>
<li><a href="https://openweathermap.org">Open Weather API</a></li>
<li><a href="https://community.algolia.com/places/">Algolia Places</a></li>
<li><a href="https://www.creative-tim.com/product/black-dashboard?_ga=2.149079679.858118941.1588541061-234568719.1584548384">Black dashboard CSS</a></li>
<li><a href="https://vuejs.org/">Vue.JS</a></li>
<li><a href="https://getbootstrap.com/">Bootstrap</a></li>
<li><a href="https://www.chartjs.org">Chart.JS</a></li>
</ul>


### Autorzy
Copyrights 2020 - Paweł Ociepka, Patrycja Wójtowicz, Adam Zaleński

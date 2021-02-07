<center> <h4 style="color: gray;">Noel Denis Rostohar</h4> </center>
<center> <h1><span style="color: #3B5998;">mStart Plus</span> - Projektni Zadatak</h1> </center>

### Live Demo: [https://mstartplus-zadatak.netlify.app/](https://mstartplus-zadatak.netlify.app/)

<br />

Instaliravanje Dependency-a

```
yarn install
```

Pokretanje Dev Servera

```
yarn serve
```

<br />

Kao što sam već napomenuo, ovo je prvi <span style="color: #42B983;">Vue</span> projekt na kojem sam radio sam od početka do kraja, tako da se ispričavam ako neke stvari nisu po best practice-u.

Također, nadam se da sam u potpunosti razumio zadatak i da sam ispunio Vaša očekivanja.

Što se tiče samog projekta, kao CSS framework koristio sam [Vueitfy](https://vuetifyjs.com/en/) - UI Library inspiriran Material Design-om. On nudi veliki broj komponenata koji su zapravo vrlo fleksibilni i prilagodljivi, te ih je vrlo lako inkorporirati u projekt.

Za podatke sam naravno koristio enpointeve navedene u samom zadatku (https://jsonplaceholder.typicode.com/ users, posts, i comments), te sam koristio axios kao HTTP client za dohvaćanje podataka.

Jedan od problema na kojeg sam naišao je bio vezan uz dohvaćanje podataka. Naime, kada se otvori određeni post, iskoči modal i započne dohvaćanje komentara koji pripadaju tome post-u. No ukoliko se modal zatvori prije nego što se komentari učitaju, sljedeći otvoreni post bi već sadržavo komentare od prethodnog post-a. To se događalo zato što funkcija koja se poziva pri zatvaranju modala reset-a komentare kao prazan array:

```
dialog.comments = []
```

no u pozadini se i dalje odvija dohvaćanje podataka. Jednom kada se dohvate, trigger-a se callback koji ponovno ispuni comments array s novodohvaćenim podacima,

```
dialog.comments = [
        {
            "postId": 1,
            "id": 1,
            "name": "id labore ex et quam laborum",
            "email": "Eliseo@gardner.biz",
            "body": "laudantium enim quasi est quidem magnam voluptate ipsam eos\ntempora quo necessitatibus\ndolor quam autem quasi\nreiciendis et nam sapiente accusantium"
        },
        ...
]
```

te se tako ti komentari prikažu prije dohvaćanja pravih komentara koji pripadaju tome post-u.
Taj problem sam riješio koristeći axios cancel token, koji, kada pozovemo cancel metodu, odmah zaustavlja njemu pripadajući fetch request.

Aplikacija je responzivna, a od dodatnih funkcionalnosti implementirao sam paginaciju post-ova, te filtriranje broja user-a koji se prikazuju.

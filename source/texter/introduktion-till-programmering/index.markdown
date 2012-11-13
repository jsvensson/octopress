---
layout: post
title: "Introduktion till programmering"
date: 2012-11-04 23:30
comments: false
sharing: true
footer: true
---

En sammanfattning av min presentation under aktivitetsveckan, och en hög länkar som förmodligen förklarar saker bättre än vad jag gjorde. Ifall ni har några frågor så kan ni nå mig via email, [johan@atomicplayboy.net](mailto:johan@atomicplayboy.net).


## Online-guider ##

* [Why's Poignant Guide to Ruby](http://mislav.uniqpath.com/poignant-guide/) -- en ganska speciell introduktion till Ruby. Med rävar. Och bacon.
* [Michael Hartl's Ruby on Rails](http://ruby.railstutorial.org/ruby-on-rails-tutorial-book) -- Rails är ett ramverk skrivet i Ruby för att utveckla web-applikationer. Jag har inte läst så mycket av den här, men den verkar också väldigt bra om man vill börja leka lite med Rails för att göra en web-app. Den går igenom både Ruby och Rails från grunden.


## Böcker ##

Ibland kan det vara bra med en bok intill skärmen.

* [Beginning Ruby](http://www.adlibris.com/se/product.aspx?isbn=1430223634) -- jag har inte läst den här själv men jag har hört den rekommenderas väldigt ofta.
* [The Ruby Programming Language](http://www.adlibris.com/se/product.aspx?isbn=0596516177) -- detta var boken jag hade med mig och några bläddrade i. Den är inte någonting man lär sig Ruby från grunden av, den är snarare en referens till språket. När man väl har lärt sig lite Ruby så är den väldigt bra för att kolla upp om det finns enklare sätt att göra saker på när man stöter på problem.


## Kommentarer

I Ruby skriver man en kommentar med tecknet `#`. Allting efter det tecknet ignoreras när man kör koden. Man kan använda detta både för att kommentera sin kod för att beskriva vad som händer, och för att kommentera bort kod som man inte vill köra just då.

``` ruby
# Säg hej till världen
print "Hello world!"

# Detta kommer inte att köras
# print "Hello world!"
```

Eftersom allting *efter* `#` ignoreras så kan man skriva en kommentar efter sin kod för att beskriva just den raden.

``` ruby
print "Hello world!"  # Säg hej
```

I väldigt många andra språk använder man två snedstreck, `//`, för att börja en kommentar. I de flesta av dessa språk kan man även köra ett block som är bortkommenterat så att man inte behöver skriva kommentartecken på varje rad. Kommentarblock börjar oftast med `/*` och avslutas med `*/`, och allting mellan dem är bortkommenterat.

{% codeblock lang:js Ett exempel i Javascript %}
// Rad 2 kommer att köras
alert("Hello");

/*
Bortkommenterat - ingenting körs här, man kan skriva
både kod och vanlig svenska
alert("Hello");
*/
{% endcodeblock %}


## Variabler

**Variabler** används för att spara information så att den blir lättare att hänvisa till när man arbetar med den. Precis som det låter på namnet så kan variabler variera -- innehållet går att ändra på.

Variabler kan heta nästan vad som helst, förutom vissa ord som är reserverade av språket man skriver i. I Ruby börjar en variabel med en liten bokstav (de kan inte börja med en siffra), och man kan använda underscores "_" för att bryta upp långa variabelnamn för att göra dem lättare att läsa. Man kan inte ha minustecken i variabelnamnet eftersom Ruby tolkar detta som ett bokstavligt minustecken och tror att man vill göra matematik.

Formellt så kallas det att "deklarera en variabel" när man ger den ett värde.

``` ruby
number = 5
my_name = "Johan"
```

Jag kan när som helst ge en variabel ett nytt värde.

``` ruby
number = 42
```

## Konstanter

En **konstant** är som en variabel -- fast tvärtom. När en konstant har blivit given ett värde så kan man inte längre ändra på konstantens värde.

I Ruby är detta inte helt sant -- man *kan* ändra på konstanten, men man får en varning om att man ändrar på en konstant när man kör koden. De flesta andra språk kommer i stället att ge ett fel och sluta köra.

I Ruby deklarerar man en konstant genom att den börjar med en stor bokstav. Vissa skriver hela namnet i stora bokstäver, men det är helt efter smak och tycke. Huvudsaken är att man håller sig till en av formerna för att undvika förvirring.

``` ruby
# Deklarera en konstant
Weekday = "lördag"

# Den här raden kommer ge en varning, vi har redan
# deklarerat konstanten Dag
Weekday = "söndag"
```


## Metoder och funktioner

Jag hann inte prata om objektorientering och det finns ganska mycket att säga om det, men här kommer en väldigt kort version. Detta kan vara ganska krångligt att förstå sig på i korthet utan att faktiskt själv sitta och experimentera med koden. Jag visar inte hur man skriver egna klasser, utan bara hur man använder några inbyggda klasser.

I objektorienterad programmering (OOP) arbetar man med **klasser** och **objekt**.

En klass är kategorin av någonting, medan ett objekt är en viss sak av den klassen. För att ta exempel från verkligheten så skulle man kunna ha en klass som heter `Inredning`, och ett bord och en stol är objekt av typen `Inredning`. De har samma klass, men de är två olika objekt.

I klassen har man kod som avgör vad objekt av den klassen har för **egenskaper** och vad de kan göra. Egenskaper är variabler som hör till objektet. Saker som ett objekt kan göra kallas för **metoder**.

Klassen `Dörr` skulle till exempel kunna ha egenskapen `namn` (så att man vet vad som finns bakom dörren), och metoderna `öppna` och `stäng`.

Man anropar egenskaper och metoder med en punkt efter objektet och sen namnet på egenskapen/metoden. Det finns väldigt många metoder för olika saker. Jag visar några metoder för objekt nedan.

Många funktioner och metoder kräver att man skriver ett eller flera **argument** till dem. I detta exemplet är strängen `Hello` argumentet man skickar till funktionen `puts`, som används för att skriva saker till skärmen.

``` ruby
puts "Hello"
```

Om man skriver flera argument separerar man dem med komman. Om `puts` får flera argument skriver den varje argument på en ny rad.

``` ruby
puts "Hello", "world"
```

I väldigt många andra språk anger man argumenten inom parenteser efter funktionsnamnet. Detta är inget krav i Ruby, men själv gör jag det väldigt ofta av gammal vana. Ibland måste man dock använda parenteser eftersom resultatet annars blir tvetydigt och Ruby ger en varning eller ett fel eftersom det inte går att tolka raden.

Observera att det inte är något mellanslag mellan funktionsnamnet och parentesen.

``` ruby
puts("Hello", "world")
```

Ibland skriver man tomma parenteser efter funktionen för att visa att det är en funktion. I det här exemplet får funktionen `puts` inga argument, så då skriver den bara en tom rad.

``` ruby
# Bägge dessa rader gör samma sak
puts
puts()
```


## Funktion eller metod? ##

Som jag skrev ovan så är en **metod** någonting som ett objekt kan göra. Skillnaden mellan en **metod** och en **funktion** är att en metod tillhör en klass, medan en funktion är fristående och inte behöver anropas av ett objekt.

Rent kodmässigt ser de likadana ut; skillnaden är att metoder skrivs inuti en klass (och hör därmed till den klassen) medan funktioner skrivs utanför en klass.

``` ruby
# Ett objekt kör metoden "length"
name.length

# En funktion med argumentet "Hello"
puts "Hello"

# En funktion som får en metod som argument
puts name.length
```


## Klasser och arv ##

Ett väldigt viktigt koncept i objektorienterad programmering är **arv**. En klass kan *ärva* från en annan klass, vilket betyder att den ärvande klassen automatiskt kan använda alla metoder och egenskaper som klassen den ärver ifrån.

Vi har till exempel klassen `Djur` där vi har en egenskap som säger att djur-objekt har fyra ben.

Om vi sen gör klassen `Katt` som ärver ifrån `Djur` så får alla katt-objekt automatiskt fyra ben.

Sen gör vi klassen `Spindel` som också ärver från `Djur`. Vi kan ändra egenskapen och säga att spindel-objekt ska ha åtta ben, men allt annat som finns i `Djur` kan användas som vanligt.

Arv är väldigt viktigt eftersom det gör att man väldigt enkelt kan återanvända sin kod, och inte behöver upprepa samma kod flera gånger -- vill jag lägga till fler djur-klasser behöver de bara ärva från `Djur`, så behöver jag bara skriva ny kod för det som är speciellt för just det djuret.


## Datatyper

Datatyper är saker som man kan spara i variabler och konstanter. Det finns fyra grundläggande datatyper i Ruby: siffror, strängar, samlingar och ordlistor.

I Ruby är dessutom datatyperna **objekt** av en viss **klass**.

Ruby har en femte datatyp som kallas symboler, men de är rätt speciella och jag kommer inte beskriva dem här.

## Siffror

Det finns olika typer av siffror i programmering, och de har att göra med hur en siffra lagras i minnet. I Ruby behöver man väldigt sällan bry sig om typen av siffra -- Ruby konverterar automatiskt mellan de olika typerna.

Ett väldigt vanligt misstag (som jag själv snubblar på med jämna mellanrum) är tal med decimaler. Hälften av 5 är 2,5, men om man kör `5 / 2` i Ruby så får man svaret `2`. När vi läser det är det självklart för oss att det skulle ha blivit 2,5, men Ruby tittar på varje del av påståendet som en separat del -- siffran fem, ett divisionstecken och sen siffran två. Vi har bara skrivit heltal, och därför svarar Ruby med ett heltal.

Det är de två grundläggande typerna av tal -- heltal och decimaltal.

Om däremot något av talen vi försöker räkna med är ett decimaltal, så förstår Ruby automatiskt att vi vill ha decimaler. Kör vi `5.0 / 2` så får vi rätt svar, `2.5`.

Lägg märke till att man använder en punkt som decimaltecken i Ruby, precis som man gör när man skriver på engelska. Det är ett annat vanligt misstag som jag själv gör konstant.

Det finns fler typer av siffror, men de har i stort sett bara att göra med hur de lagras i minnet.

Heltal är av klassen `Fixnum`, decimaltal är av klassen `Float`.

I Ruby kan tal kan deklareras med underscores, `_`, för att göra dem mer lättlästa. De här två variablerna har samma värde:

``` ruby
num_1 = 1000000
num_2 = 1_000_000
```

## Strängar (String)

**Strängar** (klass: String) använder man för att lagra tecken som inte är ett tal -- text, helt enkelt. Strängar deklarerar man genom att ha citattecken runt innehållet.

``` ruby
my_name = "Johan"
number_as_string = "75"
empty_string = ""
```

Variabeln `number_as_string` innehåller siffror, men den är av datatypen sträng eftersom den är deklarerad så.

En sträng är fortfarande en sträng även om den inte innehåller några tecken.

Några exempel på metoder för en sträng:

``` ruby
name = "Johan"
name.length    # Svarar med hur många tecken variabeln innehåller
name.reverse   # Svarar "nahoJ"
```

## Samlingar (Array)

En **array** (klass: Array) är en lista som innehåller objekt. Den kan innehålla vad som helst, även andra arrayer -- som i sin tur kan innehålla ännu mer objekt. Man separerar sakerna i listan med ett komma.

Array betyder ungefär "samling" eller "uppsättning" på engelska, men "lista" är en bra översättning eftersom man oftast använder dem så.

En array deklareras med hakparenteser, `[ ]`.

``` ruby
empty_array = []
lotto = [4, 8, 15, 16, 23, 42]
mixed_array = [1, 2, 3, "Johan", 5, 6, empty_string]
```

Man kan även skriva listan på flera rader för att göra det mer lättläst.

``` ruby
names = [
  "Adam",
  "Bertil",
  "Caesar",
  "David"
]
```

En array har en **index** för att hänvisa till ett visst objekt i listan. Den börjar på 0 för det första objektet och ökar därifrån. Man hänvisar till en viss index med `[index]` efter arrayen:

``` ruby
names[0]  # Svarar "Adam"
names[1]  # Svarar "Bertil"
names[2]  # Svarar "Caesar"
names[3]  # Svarar "David"
```

Man kan även skriva en negativ index; då börjar den räkna från slutet av listan:

``` ruby
names[-1]  # Svarar "David"
names[-2]  # Svarar "Caesar"
```

Vill jag ändra på ett visst objekt i listan kommer jag åt det på samma sätt med en index:

``` ruby
names[0] = "Anna"  # Ändrar det första namnet i listan
```

Man kan dock inte ändra på ett objekt i listan som inte finns. Om jag tex skrev

``` ruby
names[7] = "Håkan"
```

så skulle det ge ett fel, eftersom det bara finns fyra objekt i samlingen.

För att lägga till fler namn i listan använder man metoderna `push` eller `unshift`.

``` ruby
names.unshift "Fredrik"  # Lägger till "Fredrik" först i listan
names.push "Göran"       # Lägger till "Göran" sist i listan
```

Arrayer har ganska många metoder. Om man har listan `personer` och ska dela ut en vinst till en slumpad vinnare kan man köra `personer.sample` som svarar med ett slumpat utvalt objekt ur listan.

Några andra metoder:

``` ruby
personer.sort    # sorterar listan i alfanumerisk ordning
personer.reverse # vänder listan baklänges
personer.shuffle # sorterar listan i slumpad ordning
personer.first   # första objektet i listan
personer.last    # sista objektet i listan
personer.size    # säger hur många objekt det finns i listan
```

## Ordlistor (Hash)

En ordlista (klass: Hash) är en speciell sorts samling, där man har par med värden: en **nyckel** och ett **värde**. Nyckeln måste vara unik (man kan inte ha flera nycklar med samma namn). Värdet kan vara vilket objekt som helst.

"Ordlista" är inte en helt korrekt översättning till svenska, men det passar väldigt bra för vad man använder en hash till -- man sparar information och "slår upp dem" genom att hänvisa till nyckeln. Nyckeln är ordet man letar efter i ordlistan; värdet är vad det står i ordlistan att nyckeln betyder.

En hash deklareras med klammerparenteser, `{}`. Det finns flera olika sätt att deklarera innehållet i hashen, men det vanligaste är

``` ruby
nyckel_1 => värde, nyckel_2 => värde
```

Precis som med en array kan man skriva dem på separata rader för att göra det mer lättläst. Eftersom man väldigt ofta har många saker att lagra i dem så brukar man nästan alltid deklarera dem på flera rader.

``` ruby
nyckel_1 => värde,
nyckel_2 => värde
```

Nyckelparen separeras med ett komma precis som innehållet i en array.

Om man till exempel vill göra en hash där man kan slå upp vilken klass en viss djurart hör till:

``` ruby
djur = {
  "krokodil" => "reptil",
  "gorilla" => "däggdjur",
  "manet" => "nässeldjur",
  "orangutang" => "däggdjur"
}
```

För att sedan slå upp ett djur i ordlistan:

``` ruby
djur["gorilla"]  # Svarar "däggdjur"
```

Man slår alltså upp saker med `[ ]` precis som med indexen för en array, men man använder namnet på nyckeln man vill slå upp i stället.

Man lägger till nya nyckelpar på samma sätt:

``` ruby
djur["zebra"] = "däggdjur"
```

Om nyckeln `zebra` inte finns i ordlistan så läggs den till. Om den redan finns, så kommer den få ett nytt värde.

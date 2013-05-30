#### {% title "Zastosowania różne…" %}

<blockquote>
 {%= image_tag "/images/trigrams.jpg", :alt => "[Trigrams]" %}
</blockquote>


## Generator losowych akapitów

…znany też pod nazwą generatora przemówień, to program generujący
tekst korzystający z [n-gramów](http://en.wikipedia.org/wiki/N-gram).

Użyjemy widoku CouchDB *app/markov* w którym zapiszemy
następującą funkcję map:

    :::js
    function(doc) {
      words = doc.p.match(/[\w\u00C0-\u017F\-,.?!;:'"]+/g);
      words.unshift("★", "★");
      words.push("◀");
      for (var i = 3, len = words.length; i <= len; i++) {
        emit(words.slice(i-3,i), null);
      }
    };

Jak widać, na początku akapitu dodajemy dwie „gwiazdki”
(tak zaznaczamy początek akapitu), a na końcu „trójkąt”
(a tak – koniec akapitu).
Tak wydłużony akapit dzielimy na *trigramy*, które
zapisujemy w widoku *app/markov*. Na przykład, z akapitu:

    ★ ★ - A słowo stało się ciałem! ◀

tworzymy następujące trigramy:

    ★ ★ -
    ★ - A
    - A słowo
    A słowo stało
    słowo stało się
    stało się ciałem!
    się ciałem! ◀

Teraz, poniższe zapytanie wygeneruje listę pierwszych słów akapitów:

    :::bash
    http://localhost:5984/wb/_design/app/_view/markov?startkey=["★","★"]&endkey=["★","★",{}]

Skrypt *generate-paragraph.rb* „pobiera” z widoku listę wszystkich trigramów
zaczynających się od bigramu, na przykład – `["we", "drzwiach"]`:

    {"total_rows":407268,"offset":355049,"rows":[
    {"id":"368a8d7d65caf0d871f99fb51c8aa824","key":["we","drzwiach","głównego"],"value":null},
    {"id":"1c05db1cf5b3f8137fbea24de781b5dd","key":["we","drzwiach","i"],"value":null},
    {"id":"1c05db1cf5b3f8137fbea24de7f21de7","key":["we","drzwiach","i"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51c68b507","key":["we","drzwiach","i"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51cf64351","key":["we","drzwiach","i"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51cda220d","key":["we","drzwiach","komory,"],"value":null},
    {"id":"1c05db1cf5b3f8137fbea24de7412c7e","key":["we","drzwiach","na"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51c3ddf11","key":["we","drzwiach","pojawiła"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51cf32eb0","key":["we","drzwiach","sieni,"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51cadf900","key":["we","drzwiach","sieni."],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51c5b215a","key":["we","drzwiach","spojrzał"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51c20024f","key":["we","drzwiach","ukazało"],"value":null},
    {"id":"368a8d7d65caf0d871f99fb51c8975eb","key":["we","drzwiach","widziałem,"],"value":null}
    ]}

Z pobranej listy losowany jest trigram, na przykład: `["we","drzwiach","sieni"]`.
Słowo *sieni* wypisujemy i odpytujemy ponownie widok. Ale tym razem używamy
bigramu `["drzwiach", "sieni"]`. Powtarzamy losowanie, wpisujemy wylosowane słowo itd.

Wypisywane zaczynamy od bigramu reprezentującego początek akapitu
`["★","★"]`, a Kończymy po napotkaniu trigramu zawierającego znak
końca kapitu `◀`.

    :::ruby generate-paragraph.rb
    #! /usr/bin/env ruby
    # -*- coding: utf-8 -*-
    require 'bundler/setup'
    require 'couchrest'

    class Generator
      def initialize(couch, db)
        couch = CouchRest.new("http://localhost:5984")
        @db = couch.database('wb')
      end
      def run
        bigram = ["★", "★"]
        while true
          bigram = probable_follower_for(bigram)
          if bigram[1] == "◀"
            break
          else
            print "#{bigram[1]} "
          end
        end
      end
    private
      def probable_follower_for(start)
        view = @db.view('app/markov', startkey: start, :endkey=>[start, {}].flatten)
        row = view['rows'].sample # get random row (Ruby v1.9.2+)
        return row['key'][1,2]
      end
    end

    generator = Generator.new("http://localhost:5984", "wb")
    generator.run

Skrypt uruchamiamy tak:

    :::bash
    ./generate-paragraph.rb | fmt

Oto wygenerowany akapit:

    Pan Czarniecki umyślnie podesłał języka, aby nas w Suchej
    obtarli. Wielu go widziało.


### Natural Language Corpus Data

* [Google Books Ngram Viewer](http://storage.googleapis.com/books/ngrams/books/datasetsv2.html)
* Peter Norvig, [Natural Language Corpus Data](http://norvig.com/ngrams):
  - [shakespeare.txt](http://norvig.com/ngrams/shakespeare.txt) –
  - [Natural Language Corpus Data](http://norvig.com/ngrams/ch14.pdf)


### Unicode Tables

* [Unicode Table](http://www.tamasoft.co.jp/en/general-info/unicode.html)
* U+2605 (★), U+25C0 (◀); see [Miscellaneous Symbols](http://www.unicode.org/charts/PDF/U2600.pdf)


<blockquote>
 {%= image_tag "/images/peter_norvig.jpg", :alt => "[Peter Norvig]" %}
 <p>
  Current systems are more likely to be built from examples than from
  logical rules. Don’t tell the computer how an expert solved a
  problem. Rather, give it lots of examples of what past problems have
  been like: Give it the features that describe the initial situation, a
  description of the solution used and a score indicating how well the
  solution worked.
 </p>
 <p class="author">– <a href="http://www.nypost.com/p/news/opinion/opedcolumnists/the_machine_age_tM7xPAv4pI4JslK0M1JtxI#ixzz1GsAu6C00">Peter Norvig</a> (Google)</p>
</blockquote>

# Twitter – wizualizacja statusów

Liczba dnia: 572,000 – liczba kont utworzonych 12.03.2011 na *twitter.com*.

Liczba tweets wysyłanych jednego dnia:

* 50,000,000 – w lutym 2010 roku
* 140,000,000 – w lutym 2011 roku

([źródło](http://stephenslighthouse.com/2011/03/15/twitter-stats/))

Innymi słowy, w każdej sekundzie wysyłanych jest mniej więcej 1620 tweets.


## Przeglądanie 10 a 10,000 tweets

Codziennie z rana przeglądam to co się ostatnio wydarzyło:
czy ukazała się nowa wersja którejś z baz,
modułów, bibliotek itp. W tym celu „zaprzyjaźniłem się”
(tak to się nazywa na Twitterze) z autorami narzędzi
z których korzystam na codzień.
Wymaga to ode mnie przeczytania maksymalnie kilkunastu
tweets i przejrzenia kilku stron.

Ostatnio spróbowałem określić swoje zainteresowania w kilku słowach. Oto one:

    :::javascript tracking
    track=infochimps,elasticsearch,couchbase,couchdb,\
      mongodb,redis,neo4j,hadoop,cassandra,nodejs,\
      github_js,github_rb,rails

Twitter umożliwia, przy pomocy
[POST statuses/filter](https://dev.twitter.com/doc/post/statuses/filter) API,
zbieranie (*harvesting*) statusów zawierających jedno z tych słów.

To polecenie:

    curl -d @tracking http://stream.twitter.com/1/statuses/filter.json -K credentials

wypisuje na *stdout* pobrane tweets. Plik *credentials* użyty powyżej zawiera:

    user = Login:Password

Moje statuses/tweets spływają w tempie około 10 na minutę. W tym samym czasie,
wszystkich wysłanych tweets jest około 100,000 (60\*1600=96,000).
Oznacza to, że interesujące mnie rzeczy zawiera mniej więcej jeden
tweet na 10,000.

W ciągu jednego dnia moge zebrać w przybliżeniu 15,000
(10\*60*24=14,400) tweets. Oczywiście w pobranych tweets będzie sporo
śmieci.  Zwłaszcza w tweetach z Cassandrą i Rails.

Zakładając, że przejrzenie jedenego tweet zajmuje 1–2 sekundy,
przeglądnięcie wszystkich zebranych w jeden dzień, zajęłoby mi około
3–6 godzin plus godzina ekstra na prześledzenie tych interesujących.
Dlatego przeglądanie wszystkich tweets nie wchodzi w grę.

Czy można sobie jakoś poradzić z tym nadmiarem informacji?
Po krótkim zastanowieniu się, doszedłem do wniosku,
że wielokrotnie retweeted tweets, mogą być warte obejrzenia
i poczytania.

Jak wyglądają retweeted tweets?
Przejrzałem kilkudziesiąt z nich i moją uwagę
zwrócił powtarzający się schemat:

    'bout time! RT @rbates @dhh: Rails 3.1 will ship with jQuery...
    ...a real time chat app" http://t.co/GkzQgwp (via @dalmaer)
    ...Cassandra Wilson - Harvest Moon http://t.co/GguTS9v via @youtube
    Is on the phone with Cassandra - via http://truecaller.com

Aby się bliżej temu przyjrzeć skorzystałem następującego widoku:

    :::javascript
    function(doc) {
      if (doc.text.match(/\b(RT|via)\s*@\b/))
        emit(doc.text, null);
    }

Widok ten usuwa ok. 3/4 tweets. Zostaje 1/4.

Na razie zgromadziłem ok. 20 tys. tweets.
Oznacza to, że pozostaje z grubsza 5 tys. do przejrzenia.
Ciągle jest tego za dużo.


## Odchudzanie danych

Ponieważ zgromadzone dane zajmują 60 MB, co przekłada się na czas
wykonywania widoków, więc aby skrócić ten czas, postanowiłem
„odchudzić” dane.

Po przyjrzeniu się pojedynczemu tweet, postanowiłem, że z każdego
tweet zostawię tylko:

    _id
    created_at
    entities
    text
    user.screen_name
    user.lang

oraz dorzucę tablicę z datą w formacie:

    created_on: [2011, 3, 16, 22, 28, 32]

Do czyszczenia danych oraz wrzucenia oczyszczonych danych do bazy
wykorzystałem następujący skrypt:

    :::ruby massage-tweets.rb
    #!/usr/bin/env ruby
    # -*- coding: utf-8 -*-
    if RUBY_VERSION < "1.9.0"
      require 'rubygems'
    end
    require 'date'
    require 'couchrest'
    require 'pp'

    db = CouchRest.database("http://127.0.0.1:5984/nosql")
    pager = CouchRest::Pager.new(db)
    out = CouchRest.database!("http://127.0.0.1:5984/nosql-slimmed")

    pager.all_docs do |slice|
      chunk = slice.map do |element|
        doc = db.get(element["id"])

        date = DateTime.parse(doc['created_at'])
        created_on =[date.year, date.month, date.day, date.hour, date.minute, date.second]
        {
          "_id" => doc["_id"],
          "text" => doc["text"],
          "entities" => doc["entities"],
          "screen_name" => doc["user"]["screen_name"],
          "lang" => doc["user"]["lang"],
          "created_at" => doc["created_at"],
          "created_on" => created_on
        }
      end
      out.bulk_save(chunk)
    end

Teraz możemy się bliżej przyjrzeć tweets. Na początek, dokładniejsze oszacowanie
tweets per hour. Taki widok powinien wystarczyć, z funkcja map:

    function(doc) {
        emit(doc.created_on, null);
    }

oraz wbudowaną funkcją reduce *_count*.

Rezultaty z zaznaczonym grouping level 4:

    [2011, 3, 17, 20] – 255
    [2011, 3, 17, 19] – 291
    [2011, 3, 17, 18] – 209

Co daje 4–5 tweets na minutę, czyli dwa razy mniej niż pierwsze oszacowanie.
Pozostaje około 2000 tweets do codziennej lektury.
Ciągle jest tego za dużo.


## Nowe widoki

Najwyższa pora aby przyjrzeć się najczęściej cytowanym autorom.
W tym celu, z pola *text* każdego tweeta wyciągnę *@name* występujące
w kontekście:

    RT @name
    via @name

Dodatkowo dodam do widoku pole *user.screen_name*,
czyli login użytkownika, który retweeted tweet
(a nie *user.name* – czyli imię i nazwisko).

Widok zapiszę w bazie za pomocą programu *couchapp* dla NodeJS.
Tak programujemy widok w Javascript:

    :::javascript sun.js
    var couchapp = require('couchapp');

    ddoc = {
        _id: '_design/test'
      , views: {}
    }
    module.exports = ddoc;

    ddoc.views.sun = {
      map: function(doc) {
        var retweeted = /\b(via|RT)\s*(@\w+)/ig;
        // w tej wersji tylko pierwsze dopasowanie
        var match = retweeted.exec(doc.text);
        if (match != null) {
          emit([match[2].toLowerCase(), doc.screen_name], doc.text);
        };
      },
      reduce: "_count"
    }

A tak zapisujemy go w bazie:

    couchapp push sun.js http://localhost:5984/nosql-slimmed

Widok uruchamiamy w Futonie. Widok zawiera powyżej 4000 wyników.
Tym razem bez niespodzianek. Tego oczekiwałem.

Przeglądam wyniki z zaznaczonym grupowaniem na *level 1*.
Większość autorów jest raz cytowana (*retweeted*).

Sprawdzam, kilka rzeczy wielokrotnie cytowanych.

**@tenderlove** – tego gościa znam – na liczniku ma 20 cytowań.
Aby się dowiedzieć „o co chodzi” wpisuję w przeglądarce:

    http://localhost:5984/nosql-slimmed/_design/test/_view/sun?startkey=["@tenderlove"]&endkey=["@tenderlove",{}]&reduce=false

A teraz to samo, ale z wiersza poleceń:

    curl 'http://localhost:5984/nosql-slimmed/_design/test/_view/sun?startkey=\[%22@tenderlove%22\]&endkey=\[%22@tenderlove%22,\{\}\]&reduce=false'
    curl 'http://localhost:5984/nosql-slimmed/_design/test/_view/sun?startkey=\["@tenderlove"\]&endkey=\["@tenderlove",\{\}\]&reduce=false'

…no i dowiaduję się ciekawych rzeczy:

    RT @tenderlove: Looks like ruby / rails is used for emergency gas shutoff
      systems at Tokyo Gas: http://slidesha.re/hFsGY5

Przy okazji znalazłem zajmujący „rails routing quiz”:

    https://gist.github.com/871699

Ten prosty skrypt:

    :::text check.sh
    #!/bin/bash
    curl http://localhost:5984/nosql-slimmed/_design/test/_view/sun?startkey=\\[\"@$1\"\\]\&endkey=\\[\"@$1\",\\{\\}\\]\&reduce=false

powinien ułatwić przeglądanie. Teraz wystarczy wpisać na konsoli:

    ./check.sh tenderlove

Ktoś cytowany najwięcej razy, też go kojarzę – to autor biblioteki *Prototype*), na liczniku – 110:

    ./check.sh sstephenson
    "RT @sstephenson: Rails 3.1 should ship with jQuery as its default JavaScript library
    "RT @sstephenson: Rails 3.1 should ship with jQuery as its default JavaScript library
    ... i tak 110 razy ten sam tekst ...

Drugie miejsce na liście cytowań z wynikiem 41 zajmuje Ryan Bates (wszyscy go znają):

    ./check.sh sstephenson
    "Woohoo!! jQuery will be default in Rails 3.1. (via @rbates)"
    "RT @rbates: Woohoo!! jQuery will be default in Rails 3.1."
    ... i tak 41 razy ...

Na koniec inny schemat cytowań – ziteapp jest cytowany przez różne osoby:

    ./check.sh ziteapp
    ["@ziteapp","_wee_"],"value":"jQuery on Rails http://t.co/RALsAap ...
    ["@ziteapp","chrismarin"],"value":"How to Deploy a Rails app to EC2 ...
    ["@ziteapp","shimdh"],"value":"jQuery on Rails http://t.co/I3PZX2n ...
    ... większość z 19 tekstów jest różna ...


## Podsumowanie

Widok *sun* powyżej, z *group_level=1* zwraca 1749 wyników.
Ale po usunięciu tweets cytowanych raz, dwa lub trzy razy zostaje 140 wyników.
Niestety odfiltrowania niepotrzebnych rzeczy nie da zrobić za pomocą widoku.

Fajnie by było wygenerować listę posortowaną po liczbie cytowań.
Tego też nie da się zrobić za pomocą widoku!

Do takich rzeczy potrzebujemy innego mechanizmu.
W CouchDB do tych rzeczy możemy wykorzystać *list functions*.

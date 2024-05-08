<template>
  <body>
    <h1>TodoList</h1>
    <p>Jednoduchý TodoList vytvořený pomocí frameworku Vue.js. Serversite běží na Expressu.</p>
    <p>Aplikace je nasazená na Heroku pomocí free programu, kde používá pouze jeden Add-on a to ClearDB MySQL.</p>
    <p>Implementoval jsem kompletní REST API které pracuje s MySQL databází. Databáze má dva tably jeden pro ukládání users a druhý pro ukládní úkolů.</p>
    <h2>Funkce TodoListu</h2>
    <ul>
      <li>Přidání úkolu</li>
      <li>Smazání úkolu</li>
      <li>Označení úkolu jako splněný (přeškrtnutí)</li>
      <li>Editaci úkolu (jak splněného tak nesplněného) - ID úkolu je předáváno jako paramater v URL</li>
      <li>Export úkolů do JSON</li>
      <li>Filt úkolů (splněný, nesplněný, všechny) - filtr úkolů je předáván pomocí parametru v URL</li>
      <li>Přihlášení uživatele</li>
    </ul>
    <h2>Uživatelé</h2>
    <p>V zadání nebyl požadavek na registraci uživatelů tak jsem to neimplementoval. V databázi jsou pevně daní uživatelé. Jejich seznam s hesly je níže.</p>
    <ul>
      <li>email: <strong>email@email.cz</strong> heslo: <strong>email</strong></li>
      <li>email: <strong>jiri.vrany@tul.cz</strong> heslo: <strong>jirivrany</strong></li>
      <li>email: <strong>martin.hajek1@tul.cz</strong> heslo: <strong>martinhajek1</strong></li>
      <li>email: <strong>admin@admin.cz</strong> heslo: <strong>admin</strong></li>
    </ul>
    <h2>Zajímavosti</h2>
    <p>Aplikace je připravena pro práci více uživatelů najednou. Pomocí odesílání websocketů není problém s tím že pokud jeden uživatel provede změnu druhému by se změna neprojevila.</p>
    <p>Pro design aplikace jsem využíval výhradně bootsrap.</p>
    <h2>Limity Heroku</h2>
    <p>Heroku obsahuje nějaké limitující pravidla či vlastnolsti které se mi na mém lokálním stroji neprojevovali.</p>
    <p>Prvním limitem je že server musí každou vteřinu posílat na websocket server zprávu (interval by možná mohl být delší). Je to z důvodu že po cca 30 vteřinách když se client odpojí od aplikace Heroku si myslí že apliakce padla protože websocket server je nečinný a spadne s chybou H15.</p>
    <p>Dále jsem musel změnit připojení z klasické na pool, protože aplikace po odpojení ztratila spojení s Heroku ClearDB databází a spadla.</p>
    <p>Client site i Server site běží na jedné URL (mělo by to běžet na oddělených strojích) nicméně nevím jak by to bylo s free plánem od Heroku (ačkoliv je add-on ClearDB zadarmo Heroku po vás žádá platební kartu přidat a nerad bych aby mi účtovali za druhou aplikaci).</p>
    <p>Jelikož je jakkákoliv změna zpracována odesláním websocketu občas trošičku déle trvá než se změna projeví (např. při odškrtnutí úkolu). Je to nejspíše z důvodu pomalejší odezvy websocketu jelikož free plan asi nenabízí nejvyšší rychlosti. Na localehostu to funguje okamžitě.</p>
    <p>Heroku aplikaci uspí po nějaké době nečinosti. První připojení může být tedy delší než normálně.</p>
    <h2>Návod</h2>
    <h3>Přihlášení</h3>
    <p>Na stránce https://wea-todolist.herokuapp.com/#/login se přihlásíte pomocí pevně nastavených klientských účtů, které jsou uvedené výše.</p>
    <h3>Operace s úkoly</h3>
    <p>Pro vtyvoření nového úkolu je nutné napsat název úkolu do textového pole a myší stisknout tlačítkop Add. Úkol se automaticky přidá do seznamu úkolů a nastaví se jako nesplněný.</p>
    <p>Pro označení úkolu jako splněný je nutné kliknout na checkbox. Úkol který jste již označili jako dokončený lze opět "odkliknout" a znovu označit jako nesplněný.</p>
    <p>Pro smazání úkolu (jak splněného tak nesplněného) je nutné kliknout na <b-icon icon="trash" class="remove mdi"></b-icon>. Tímto odstraníte úkol. Tato operace je nevratná!</p>
    <p>Pro editaci úkolu je nutné kliknout na <b-icon icon="pencil" class="edit mdi"></b-icon>. Poté se otevře okno ve kterém bude textové pole s aktuálním popisem úkolu. Tento text je možné upravit a následně stisknout OK pro uložení. V případě že budete chtít akci zrušit můžete stisknout tlačítko Cancel.</p>
    <h3>Použití filtrů</h3>
    <p>Nad seznamem úkolů je filtr pro filtrování úkolů. Defaultně je nastaven že zobrazuje všechny úkoly. Lze si zvolit zda chcete zobrazit úkoly splněné nesplněné nebo všechny.</p>
    <h3>Export do JSON</h3>
    <p>Pod seznamem úkolů je tlačítko JSON. Po kliknutí na něj se otevře nové okno se seznamem úkolů v JSON formátu. Exportuje všechny úkoly (aktuálně aplikovaný filtr na tento export nemá vliv).</p>
    <h3>Tlačítko About</h3>
    <p>Vede na stránku na které se právě nacházíte. Stránka slouží jako taková náhrada Readme.</p>
  </body>
</template>

<script>
export default {
  name: "AboutSite",
  data() {
    return {
    };
  },
};
</script>
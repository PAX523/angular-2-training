```
Angular 2 Grundkurs

https://www.linkedin.com/learning/angular-2-grundkurs/
======================================================

Polyfills (für Browser-Unterstützung älterer Browser)

	core js (ES2015/ES6 Polyfills)
	
Libraries

	RxJS (Ereignisse/Prozesse überwachen, für HTTP-Aufrufe durch Angular genutzt)
	
	zone.js (Ausführungskontext ermöglicht, die Ausführung zu überwachen/steuern)
	
	reflect-metadata (um Meta-Daten konsistent zu einer Klasse hinzuzufügen)
	
	systemjs (Modul-Lader)

Standardmodule in Angular

	@angular/core
	
		Immer notwendig
		
		Kern für Komponenten, Direktiven, Dependency Injection, Komponentenlebenszyklus
	
	@angular/common
	
		Bereitstellung von grundlegenden Direktiven, Pipes, Services
	
	@angular/compiler
	
		Kombiniert Logik mit Vorlagen
		
		Compiler wird automatisch über platform-browser-dynamic angestoßen
	
	@angular/platform-browser
	
		Browser-/DOM-Bestandteile
		
		Rendern neuer Elemente
	
	@angular/platform-browser-dynamic
	
		Compiler anstoßen, um View zu aktualisieren
		
		Initialisierung meiner Anwendung
	
	@angular/http
	
		Für HTTP-Aufrufe
	
	@angular/router
	
		Komponenten-Routing

Module

	Bündeln Funktionen/Features und erlauben Verteilung dieser + Wiederverwendung
	
	Vergleichbar mit JARs in Java
	
	Angular-eigene Module:
	
		BrowserModule (Events, DOM)
		
		CommonModule (Direktiven, Pipes)
		
		HttpModule (XHR-Aufrufe)
		
		FormsModule (Formulare)
		
		RouterModule (komponentenbasiertes Routing)

	Modul anlegen:
	
		@NgModule({
			imports: [ BrowserModule ],				// für Imports von anderen Modulen
			declarations: [ AppComponent ]			// für Nutzung von anderen Klassen
		})
		export class AppModule {}

Komponenten

	Kann ich in meiner Anwendung einbinden
	
	Logik wird in einer Klasse beschrieben, z. B.:
	
		class AppComponent {
			constructor() {
				console.log("instantiated");
			}
		}
	
	Eigenschaften und Methoden lassen sich binden:
	
		export class AppComponent {
			name: string = "Hallo Welt";
			
			onClick() {
				...
			}
		}
	
	Visualisierung wird als HTML beschrieben. Bindung auf Eigenschaft wird mit geschweiften Klammern und Name der Eigenschaft gemacht:
	
		<h1>{{name}}</h1>
	
	Ereignis wird als Attribut in runden Klammern definiert: (ereignisname)="..."
	Handler wird namentlich dem Attribut zugewiesen:
	
		<h1 (click)="onClick()">{{name}}</h1>
	
	Komponenten lassen sich auch verschachteln:
	
		<h1>...</h1>
		<weitere-komponente></weitere-komponente>
		
		______________________________________________
		
		@NgModule({
			imports: [ BrowserModule ],
			declarations: [ AppComponent, WeitereKomponente ]
		})
		export class AppModule {}
	
	Für Initialisierung muss in Klassendeklaration die Root-Klasse angegeben werden, auf die alles aufbaut:
	
		@NgModule({
			imports: [ BrowserModule ],
			declarations: [ AppComponent, WeitereKomponente ],
			bootstrap: [ AppComponent ]				// hiermit soll init beginnen
		})
		export class AppModule {}
	
	Eigenschaften von außen schreibbar machen:
	
		import {Component, Input} from "@angular/core";
		
		export class UserComponent {
			@Input("aliasForUsername")		// Dekoratorwert ist optional, ansonsten entsprecht es dem Eigenschaftsnamen, hier: "username"
			username: string;
		}
				
		<main>
			<user [aliasForUsername]=""></user>
		</main>

Direktiven

	Sind Komponenten ohne Vorlagen
	
	Können: DOM manipulieren, Funktionalität modifzieren
	
	Selektor bestimmt, wie Direktive angewandt wird:
	
		<div selector>					// beeinflusst HTML-Element ohne weitere Parameter
		
		<div [selector]="parameter">	// ...mit Parameter
	
	Hauseigene:
	
		<div *ngIf		// soll Element angzeigt werden oder nicht
		
		<div *ngFor		// wiederholtes Einfügen des Elements
		
		<a [routerLink]	// navigiere zu URL wenn auf Element geklickt wird

Pipes

	Modifiziert Ausgabe
	
	Syntax
	
		Ausdruck | Pipename : Parameter
		   ^          ^           ^
		   |          |           |
		Eingabe    zu benutz-   Parameter (falls es
		           endes Pipe   optionale Parameter gibt
	
	Beispiel: Eigenschaft "name" soll in Großbuchstaben umgewandelt werden
	
		{{name | uppercase}}
	
	Hauseigene: uppercase, lowercase, date
	
		AsyncPipe	// wartet auf Promise, bis Wert verfügbar ist
		
		CurrencyPipe	// Konvertierung in spez. Währung
		
		I18nPluralPipe	// Mehrsprachigkeit
		
		SlicePipe	// Teilmengen zurückgeben
		
			{{ getDesc() | slice : 20 }}	// Parameter ist Startindex
			{{ getDesc() | slice : 0 : 30 }}	// von Startindex bis Endindex (Parameter werden immer mit Doppelpunkt getrennt)
			{{ getDesc() | slice : 20 | uppercase }}	// Verkettung von Pipes
			
Datenbindung

	Template-Ausdrücke
	
		<h1>{{instanz.eigenschaft}}</h1>
	
	Werte
	
		Eigenschaft: <img [src]="imageUrl">
		Attribut: <button [attr.aria-label]="help">
		Klassen: <div [class.active]="isActive">	// CSS-Klasse "active" soll hinzugefügt werden, wenn in der Komponente die Eigenschaft "isActive" true ist
		Style: <div [style.color]="colr">			// setze Style "color" auf den Wert von "colr", der in meiner Komponente als Wert hinterlegt worden ist
		
		Hinweis: Möchte man Wert von Attributen mit Inhalt aus der Komponente mischen, geht das mit geschweiften Klammern innerhalb des Wertes:
		
			<p title="Hallo {{eigenschaftsname}}!">
		
		Möchte ich es ohne geschweifte Klammer machen geht es so:
		
			<p [title]="'Hallo '+eigenschaftsname+'!'">
		
		Verändere ich CSS-Styles, so kann man das machen:
		
			<p [style.margin]="margineigenschaftsname+'px'">
		
		Einheitsangabe lässt sich auch direkt definieren (hier: Prozent):
		
			<p [style.margin.%]="margineigenschaftsname">
	
	Ereignisse
	
		<button (click)="onClick()">	// wenn geklickt wird, führe Methode "onClick" in meiner Komponente aus
	
	Template-Operation
	
		{{num+1}}		// addiere "1" auf den Wert der Eigenschaft "num" in meiner Komponente und gebe dies aus
	
	Variablendefinition
	
		<input type="number" #num1 [value]="23">
		<h2>{{num1.value}}</h2>

Dependency Injection

	In Konstruktorparameter bei Instanziierung erzeugt Angular automatisch Instanzen:
	
		constructor(http: Http) {
			console.log(http);
		}
		
		Angular schaut im Injector-Container nach einer Instanz, die vom Type "Http"-Service ist und nimmt diese
	
	Voraussetzung:
	
		Vor Nutzung im Modul einbinden, damit diese im Container des Injectors abgelegt werden
		
		@NgModule({
			imports: [..., HttpModule]		// "HttpModule" führt dazu, dass ich für DI den Http-Service zur Verfügung habe
		})
		export class AppModule {}
	
	Für eigene geschriebene Services, muss die Klasse des Service hier angegeben werden:
	
		@NgModule({
			imports: [...],
			providers: [MyService]
		})
		export class AppModule {}
	
	Service ist Singleton, Ausnahme: lazy loaded Module (z. B. für Services, die erst benötigt werden, wenn Nutzer bestimmten Bereich der Anwendung betritt)
	
	Ich verwende niemals irgendwo den new-Operator, um etwas zu instanziieren, sondern überlasse das der DI

Services

	View-unabhängige Business-Logik
	
	Wird gebraucht, wenn ich die Logik in mehreren Komponenten brauche, z. B. Login-Service
	
	Komponenten sollte nur View-spezifische Logiken haben,
	Services haben View-unabhängige Logiken
	
	Wenn ein Service auf andere Services zugreifen möchte, braucht er den "Injectable"-Dekorator

Router

	Basis einer Single Page Application
	
	Bestimmt, welche Komponente in welchem Status meiner Anwendung angezeigt wird
	
	Status wird über eindeutige URL charakterisiert

======================================================

Node-Versions-Manager installieren (global mit "g"):

	npm install n -g

Zusätzliche Version von Node.js installieren:

	n 5.4.0

Node.js-Version umändern:

	n

======================================================

NEU: Starter-Projekt erzeugen

	npx -p @angular/cli ng new hello-world-project
	
	npm start -- --open

Projekt-Template erzeugen (veraltet)

	npm init
	
	npm install concurrently --save-dev	# um 2 Node-Kommandos gleichzeitig auszuführen (nur Abhängigkeit für Entwicklungszeit)
	
	npm install lite-server --save-dev	# für Projektausführungsvorschau
	
	npm install typescript --save-dev	# für Kompilierung des Projekts
	
	npm install typings --save-dev		# für die gängigen Typendefinitionen in TypeScript
	
	# Polyfills und Vendor-Bibliotheken
	
	npm install core-js --save			# diverse Polyfills für ES2015
	
	npm install rxjs --save				# Bibliotheken zur Überwachung von Ereignissen + asynchronen Prozessen
	
	npm install zone.js --save			# Ausführungskontext überwachen + steuern
	
	npm install reflect-metadata --save	# Metadaten konsistent zur einer Klasse hinzufügen
	
	npm install system.js --save		# ES2015-Module problemlos im ES5-Kontext laden können
	
	# Angular-Module
	
	npm install @angular/core --save	# Hauptbestandteile für Komponenten/Direktiven
	
	npm install @angular/common --save	# z. B. ngIf-Direkte etc.
	
	npm install @angular/compiler --save	# Verbindung von Logik und View
	
	npm install @angular/platform-browser --save	# Kompilierung
	
	npm install @angular/platform-browser-dynamic --save	# Interaktion mit nativen Browser-Events
	
	npm install @angular/http --save	# für XHR-Requests
	
	npm install @angular/router --save	# für Routing

Konfiguration

	Um TypeScript-Compiler ansprechen zu können, in package.json folgendes Script hinzufügen:
	
		"scripts": {
			...
			"tsc": "tsc"
		}
	
	Kompilieren später mit:
	
		npm run tsc
	
	TypeScript-Konfiguration tsconfig.json anlegen:
	
		npm run tsc -- -init		# "--" sagt aus, die nächsten Optionen werden nicht an npm, sondern an tsc übergeben
	
	In "compilerOptions" von tsconfig.json eintragen:
	
		"moduleResolution": "node",
		"emitDecoratorMetadata": true,
		"experimentalDecorators": true
	
	...d.ts-Dateien enthalten Informationen, wie Klasse sich zusammensetzt, welche Methoden sie hat, welche Typen es gibt, wie Parameter typisiert sind
	
	Für Ansprechen von Typings, in package.json folgendes Script hinzufügen:
	
		"typings": "typings"
	
	Typings-Konfiguration typings.json anlegen:
	
		npm run typings -- init
	
	Alle Typendefinitionen installieren:
	
		npm run typings -- install dt~node --save --global
		npm run typings -- install dt~core-js --save --global
	
	Installationen von allem automatisieren (für "npm install"), indem in package.json folgendes Script hinzugefügt wird:
	
		"postinstall": "typings install"
	
	Konfiguration system.config.js für System.js hier herunterladen: https://github.com/netTrek/angular-2-training
	
	index.html herunterladen und ablegen
	
	Datei anlegen unter: /app/main.ts
	
	Um Ausführung mit Hotreload zu unterstützen, in package.json folgendes Script hinzufügen:
	
		"scripts": {
			"start": "tsc && concurrently \"npm run tsc:w\" \"npm run lite\"",
			...
			"tsc:w": "tsc -w",
			"lite": "lite-server"
		}
	
	Projektausführung:
	
		npm start

======================================================

Komponenten: /app/app.component.ts

	import {Component} from "@angular/core";

	@Component({
		selector: 'app',					// erlaubt mir später Zugriff auf diese Komponente via diesem Namen
		template: '<h1>Hello World</h1>'
	})
	export class AppComponent {}
	
	Für eigene Komponenten (z. B. User-Komponente) hier ablegen: /app/user/user.component.ts
	
		import {Component} from "@angular/core";

		@Component({
			selector: 'user',
			template: '<header>....Lorem ipsum'
		})
		export class UserComponent {}
	
	Anschließend muss Root-Komponente mitgeteilt werden, dass es mit dieser Komponente zusammenarbeiten soll:
	
		import {UserComponent} from "./user/user.component";
		
		@Component({
			...
			directives: [UserComponent]		// Zusammenarbeit mit diesen Direktiven + Komponenten
		})
		export class AppComponent {}
		
	Alternativ kann dies auch in app.module.ts über "declarations" gemacht werden
	
	Wenn man diese Komponente für außen verfügbar machen möchte, muss man für diese auch wieder eine user.module.ts anlegen. Dann brauche ich für die Nutzung im AppModule nicht die Komponente unter "declarations" aufführen, sondern das Modul in "imports".
	
			import {CommonModule} from '@angular/common'
			...
			@NgModule ( {
				imports: [CommonModule],		// für wichtige Direktiven + Pipes
				declarations: [UserComponent],
				exports: [UserComponent]		// damit es in AppModule importiert werden kann
			})
			export class UserModule {}

Module: /app/app.module.ts

	Enthält nur Klassenexporte mit leeren Körpern, die mit Meta-Daten angereichert sind.
	
	import {NgModule} from '@angular/core'
	import {BrowserModule} from '@angular/platform-browser'
	import { AppComponent } from './app.component';
	
	@NgModule ( {
		imports: [BrowserModule],
		declarations: [AppComponent],	// referenziert die app.component.ts (siehe Import)
		bootstrap: [AppComponent]
	})
	export class AppModule {}

Komponenteninhalt (HTML) in Datei packen: /app/app.component.html

	In Komponente app.component.ts anstelle von "template" (für Direktinhalt) "templateUrl" verwenden:
	
			@Component({
				selector: 'app',
				//template: '<h1>Hello World</h1>'
				templateUrl: './app/app.component.html'
			})
			export class AppComponent {}

Styling

	In der Komponente mit "styles" oder "styleUrls":
	
		@Component({
			...
			styles: [`
				nav {
					color: black;
				}
				...
			`],
			styleUrls: ['./app/app.component.css']		// oder ausgelagerte Styles
		})
		export class AppComponent {}
	
	CSS-Klasse an eine Bedingung binden (erwartet Prädikat als Wert, der aus der Komponentenklasse kommt):
	
		<img [class.cssklassenname]="boolschereigenschaftsname">
	
	Attributwert an eine Klasseneigenschaft oder Methodenrückgabewert binden:
	
		<img [attr.aria-label]="eigenschaftsname">
		
Ereignisse

	Werden mit runden Klammern umschlossen (Parameter ist optional):
	
		<button (click)="methodenname($event)">
		
		export class ... {
			methodenname(evt: Event) {
				...
			}
		}
	
	Komponentenereignisse mit "Output":
	
		import {Component, Event, Output, EventEmitter} from "@angular/core";
		
		export class UserComponent {
			@Output			// hier kann man optional auch wieder einen Aliasnamen vergeben
			choice: EventEmitter = new EventEmitter();
			
			clickHandler(evt: Event) {
				this.choice.emit(evt);
			}
			
			selected(evt: Event) {
				...
			}
		}
				
		<button (click)="clickHandler($event)">Select</button>
		
		<user (choice)="selected($event)">

Strukturelle Diretiven

	Können DOM manipulieren
	
	Beginnen immer mit *. Folgende entfernt das Element aus DOM, wenn der Wert false ist:
	
		<img *ngIf="shouldDisplay()">
	
	Das * ist ein Shortcut für ein Template. In voller Schreibweise würde es so aussehen:
	
		<template ngIf="shouldDisplay()">
			<img>
		</template>
	
	Template in voller Schreibweise nimmt man, wenn man viel mehr und komplexere DOM-Elemente zusammen kapseln möchte
	
	Wiederholung von Elementen kann über *ngFor gelöst werden:
	
		export class AppComponent {
			userList: IUser[] = ...;
		}
		
		<user *ngFor="let usr of userList" src=".../{{usr.id}}.jpg" ...>
	
	Switch-case-Strukture wird mit *ngSwitch umgesetzt:
	
		<div [ngSwitch]="usr.pos">
			<div *ngSwitchCase="'dog'">Hund</div>
			<div *ngSwitchCase="'cat'">Katze</div>
			<div *ngSwitchDefault>Maus</div>
		</div>

Attributdirektiven

	CSS-Klasse zuweisen, wenn Bedingung erfüllt:
	
		export class AppComponent {
			selectedUser: IUser;
			
			selected(user: IUser) {
				this.selectedUser = user;
			}
		}
		
		<user 	(choice)="selected($event)"
				*ngFor="let usr of userList"
				[ngClass]="{cssklassenname: usr === selectedUser}"></user>
	
	Geht auch mit Styles:
	
		<user [ngStyle]="{margin: '5px'}">

Eigene Direktiven

	Am besten in Utils-Ordner: /app/utils/italic.directive.ts
	
		import { Directive } from '@angular/core';
	
		@Directive({
			selector: '[myItalic]'		// weil es Attributdirektive sein soll, eckige Klammern
		})
		export class MyItalic {
			@HostBinding('class.italic') isItalic: boolean = false;	// bei true wird die CSS-Klasse dem DOM-Element hinzugefügt
			
			@HostListener('mouseenter')
			onMouseEnter() {
				this.isItalic = true;
			}
			
			@HostListener('mouseleave')
			onMouseLeave() {
				this.isItalic = false;
			}
		}
		
		<user myItalic>
	
	Damit Direktive funktioniert, muss sie dem Compiler durch ein Modul bekannt gemacht werden. Am besten ein Modul für alle eigenen Direktiven anlegen: /app/utils/utils.module.ts
	
		import { NgModule } from '@angular/core';
		import { CommonModule } from '@angular/common';
		import { MyItalic } from './italic.directive';
		
		@NgModule({
			imports: [ CommonModule ],
			declarations: [ MyItalic ],
			exports: [ MyItalic ]	// damit beim Import dieses Moduls auch die Direktive dort zur Verfügung steht
		})
		export class UtilsModule {}
	
	Im AppModule dieses dann importieren bei "imports"
	
	Falls auch noch Parameter übergeben werden sollen:
	
		@Directive({
			selector: '[myColor]'
		})
		export class MyColor {
			@HostBinding('style.color') fontColor: string;	// CSS-Schriftfarbe wird immer auf diesen Wert gesetzt
			
			@Input
			set myColor(color: string) {	// bewusst gleich benannt wie der selector, damit diesem direkt der Wert zugewiesen werden kann
				this.fontColor = color;
			}
			
			get myColor(): string {
				return this.fontColor;
			}
		}
		
		<user [myColor]="usr === selectedUser ? 'red' : 'black'">

Eigene Pipes

	Am besten wieder in Util: /app/util/reverse.pipe.ts
	
		import { Pipe, PipeTransform } from '@angular/core';
		
		@Pipe({
			name: 'reverse'
		})
		export class ReversePipe implements PipeTransform {
			//transform(value: any, args: any): any {	// Maximalausprägung
			transform(value: any): any {
				if (!!value) {	// ist value überhaupt definiert?
					let isArray: boolean = value instanceof Array;
					let isString: boolean = typeof value == 'string';
					let isNumber: boolean = typeof value == 'number';
					
					if (isArray) {
						return (<Array<any>>value).reverse();
					}
					if (isNumber || isString) {
						const newVal: any = (value+'').split().reverse().join('');
										//    ^
										//   wandle in String
						
						if (isNumber) {
							return parseFloat(newVal);
						}
						
						return newVal;
					}
				}
			
				return value;
			}
		}
	
	Im UtilsModule die neue Pipe in "declarations" und "exports" aufnehmen
	
	Verwendung wie immer:
	
		<div innerText="{{usr.name | reverse}}">

Eigener Service

	Dienen dem Speichern und Verwalten View-unabhängiger Geschäftslogikinformationen (z. B. Laden/Speichern von Daten), also Informationen, die in mehreren Views verwendet werden könnten
	
	Klasse am besten hier anlegen: /app/user/user.service.ts
	
	export UserService {
		setSelectedUser(user: IUser) {
			localStorage.setItem('selected', JSON.stringify(user));
		}
		
		getSelectedUser(): IUser {
			<IUser>JSON.parse(localStorage.getItem('selected'));
		}
	}
	
	Und dann in den Modulen einbinden, wo er benötigt wird. Es wird von ihm jedoch nur eine einzige Singleton-Instanz registriert:
	
		@NgModule({
			...
			providers: [UserService]
		})
		export class UserModule {}
	
	Wo ich den Service brauche, kann ich ihn mir über den Konstruktor injecten lassen:
	
		@Component(...)
		export class AppComponent {
			constructor(private userService: UserService) {	// "private" führt dazu, dass der Parameter implizit als Eigenschaft hinterlegt wird: "this.userService"
				...
			}
		}
	
	Möchte man, dass der eigene Service auch andere Services injeziert bekommen kann, muss man ihn hiermit dekorieren:
	
		import { Injectable } from '@angular/core';
		
		@Injectable
		export class UserService {
			constructor(serv: AnotherService) {}
		}
	
	Einfache Werte via DI bereitstellen:
	
		@NgModule({
			...
			providers: [..., {provide: 'AUTHOR', useValue: 'Saban'}
			]
		})
		export class UserModule {}
		
	Und injecten lassen:
	
		export class AppComponent {
			constructor(@Inject('AUTHOR') public author: string) {
				...
			}
		}
	
	Aber Achtung: Setze ich diesen Wert unter Providers in anderen Modulen ebenfalls, wird er überschrieben (das aller letzte Modul gewinnt)
	
	Der Wert kann mit "multi" auch als Liste behandelt werden lassen, so dass bei mehrfachem Setzen die ganzen Werte in die Liste hinzugefügt werden (abhängig von Reihenfolge der Module):
	
		providers: [..., {provide: 'AUTHOR', useValue: 'Saban', multi: true}
		
		providers: [..., {provide: 'AUTHOR', useValue: 'Franz', multi: true}
		
		Dann ist der Wert von "AUTHOR": ["Saban", "Franz"]

Routing

	Vorbereitung: Komponenten für alle Views schaffen
	
	In index.html im head-Element die Basis-URL für alle URLs definieren:
	
		<base href="/"/>
	
	Wenn die Basis-URL ein längerer Pfad ist (z. B. "/platform/app/"), dann muss der Server auch so konfiguriert werden, dass er nicht nach den Ordnern "platform/app" sucht, sondern den Root-Ordner nimmt
	
	Eine Routing-Konfiguration hier erstellen: /app/app.routing.ts
	
		import { Routes, RouterModule } from '@angular/router';
		import { ModuleWithProviders } from '@angular/core';
		...
		
		const appRoutes: Routes = [
			{ path: '', pathMatch: 'full', redirectTo: 'home' },	// wenn kein Pfad eingegeben wurde, dann leite auf home weiter; lass ich diese Angabe weg, würde "router-outlet" leer bleiben
			{ path: 'home', component: HomeComponent },		// Pfad für Home
			{ path: 'list', component: UserListComponent },	// Pfad für die Liste
			{ path: '**', redirectTo: 'home' }		// muss zu allerletzt stehen; verhindert 404 bei ungültigen URLs und leitet auf home weiter
		]
		
		export const routing: ModuleWithProviders = RouterModule.forRoot(appRoutes);
	
	Soll ein Link auch noch Parameter haben, so werden sie in der Form definiert:
	
		...path: 'list/:id'...
	
	Und im Applikations-Modul den Routing-Provider einbinden:
	
		...
		@NgModule({
			...
			imports: [
				...
				routing
			]
		})
		export class AppModule {}
	
	Haupt-HTML-Dokument "app.component.html" mit Container-Element für den Router versehene, das er immer entsprechend befüllt:
	
		...
		<main>
			<router-outlet/>
		</main>
		...
	
	Verlinkungen auf die URLs setzen:
	
		<a routerLink="/home" routerLinkActive="cssKlassennameBeiAtkivenLinks">Home</a>
		<a routerLink="/list" routerLinkActive="cssKlassennameBeiAtkivenLinks">Liste</a>
	
	Programmatisch mit dem Router-Service navigieren:
	
		...
		export class HomeComponent {
			constructor(private router: Router) {}
			
			goList() {
				const link = [ '/list' ];	// Array, weil bei Parametern im Link diese dort kommagetrennt angegeben werden: [ '/list', 33 ]
				
				this.router.navigate(link);
			}
		}
```

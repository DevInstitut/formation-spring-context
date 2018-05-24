# TP #1 - Configuration XML


## Partie 1 - Prise en main Spring Core

### Github

* Créer un fork du projet `sirh-gestion-paie`.
* Importer le projet sur votre poste.
* Ouvrir STS (Spring Tools Suite).
* Importer le projet Maven `sirh-gestion-paie`.

### Intégration du BOM Spring

Le projet Spring est constitué de nombreux sous-projets avec chacun son cycle de vie.
Pour éviter d'utiliser deux sous-projets avec des versions incompatibles, le projet met à disposition un BOM (Bill Of Materials).
Il s'agit d'une dépendance Maven spéciale à intégrer avec le scope _import_. 

* Compléter le projet dans la section _dependencyManagement_ avec :


```xml
<project ...>
	
	<properties>
		...
	</properties>

	<build>
		...
	</build>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-framework-bom</artifactId>
				<version>4.3.8.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
</project>
```

### Intégration Spring Context

Le projet _spring-context_ contient les outils permettant de créer le contexte Spring.

* Ajouter la dépendance vers _spring-context_ :

```xml
<project ...>
	...
	<dependencyManagement>
		...
	</dependencyManagement>

    <dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
		</dependency>
	</dependencies>
</project>
```

### Classe PaieUtils

Dans le cadre de cette application, nous allons manipuler des types _java.math.BigDecimal_.
Les nombres affichés auront toujours 2 chiffres après la virgule.

Centralisons cette logique dans une classe _dev.paie.util.PaieUtils_.

* Créer une classe _dev.paie.util.PaieUtils_ avec le contenu suivant :

```java
public class PaieUtils {

	/**
	 * Formate un nombre sous la forme xx.xx (exemple : 2.00, 1.90). L'arrondi
	 * se fait en mode "UP" => 1.904 devient 1.91
	 * 
	 * @param decimal nombre à formater
	 * @return le nombre formaté
	 */
	public String formaterBigDecimal(BigDecimal decimal) {
		DecimalFormat df = new DecimalFormat();
		// forcer le séparateur "." même sur un poste en français
		df.setDecimalFormatSymbols(DecimalFormatSymbols.getInstance(Locale.UK));
		df.setMaximumFractionDigits(2);
		df.setRoundingMode(RoundingMode.UP);
		df.setMinimumFractionDigits(2);
		df.setGroupingUsed(false);
		return df.format(decimal);
	}

}
```

Nous allons à présent créer une classe de test unitaire pour valider le fonctionnement de la méthode _formaterBigDecimal_.

* Dans la partie _src/test/java_, créer une classe _dev.paie.util.PaieUtilsTest_ avec le contenu suivant :

```java
public class PaieUtilsTest {
	
	private PaieUtils paieUtils;

	@Before
	public void onSetup() {
		// code exécuté avant chaque test
	}

	@Test
	public void test_formaterBigDecimal_entier_positif() {

		String resultat = paieUtils.formaterBigDecimal(new BigDecimal("2"));

		assertThat(resultat, equalTo("2.00"));

	}

	@Test
	public void test_formaterBigDecimal_trois_chiffres_apres_la_virgule() {

		String resultat = paieUtils.formaterBigDecimal(new BigDecimal("2.199"));

		assertThat(resultat, equalTo("2.20"));

	}

	@After
	public void onExit() {
		// code exécuté après chaque test
	}
}
```

* Exécuter le test. Vous devriez avoir un _NullPointerException_ du fait que _paieUtils_ n'est pas valorisé.

### Première configuration Spring

Nous allons à présent récupérer une instance de _paieUtils_ via une configuration Spring.

* Créer un fichier de configuration Spring _src/main/resources/app-config.xml_ (clic droit > _Spring Bean Configuration File_).
Ajouter le contenu suivant qui ajouter un bean appelé _paieUtils_.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="paieUtils" class="dev.paie.util.PaieUtils"></bean>

</beans>
```

Dans la classe de test _dev.paie.util.PaieUtilsTest_ :

* Ajouter un champ _context_ :

```java
private ClassPathXmlApplicationContext context;
```

* Compléter la méthode _onSetup_ :

```java

@Before
public void onSetup() {
	context = new ClassPathXmlApplicationContext("app-config.xml");
	paieUtils = context.getBean(PaieUtils.class);
}
```

* Compléter la méthode _onExit_ :

```java
@After
public void onExit() {
    context.close();
}
```

* Vérifier que le test est passant.

* Ajouter un autre cas de test.


## Partie 2 - Jeux de données

Construisons à présent un jeux de données avec Spring.

* Visualiser les fichiers _cotisations-imposables.xml_ et _cotisations-non-imposables.xml_ :

```
/src
    /main
        /resources
            cotisations-imposables.xml
            cotisations-non-imposables.xml
```

Ils contiennent les définitions des cotisations.

Créons à présent un objet _bulletin1_ qui représenterait un jeux de données complet.

image::images/entites.png[]


* Créer un test unitaire _dev.paie.util.JeuxDeDonneesTest_ :

```java
public class JeuxDeDonneesTest {

	private ClassPathXmlApplicationContext context;
	private BulletinSalaire bulletin1;

	@Before
	public void onSetup() {
		context = new ClassPathXmlApplicationContext("jdd-config.xml");
		bulletin1 = context.getBean("bulletin1", BulletinSalaire.class);
	}

    @Test
	public void test_primeExceptionnelle() {
		assertThat(bulletin1.getPrimeExceptionnelle(), equalTo(new BigDecimal("1000")));
	}

	@Test
	public void test_employe() {
		assertThat(bulletin1.getRemunerationEmploye().getMatricule(), equalTo("M01"));
	}

	@Test
	public void test_entreprise() {
		assertThat(bulletin1.getRemunerationEmploye().getEntreprise().getSiret(), equalTo("80966785000022"));
		assertThat(bulletin1.getRemunerationEmploye().getEntreprise().getDenomination(), equalTo("Dev Entreprise"));
		assertThat(bulletin1.getRemunerationEmploye().getEntreprise().getCodeNaf(), equalTo("6202A"));
	}

	@Test
	public void test_cotisationsNonImposables() {
		List<Cotisation> cotisationsNonImposables = bulletin1.getRemunerationEmploye().getProfilRemuneration()
				.getCotisationsNonImposables();
		Stream.of("EP01", "EP02", "EP03", "EP04", "EP05", "EP06", "EP07", "EP12", "EP19", "EP20", "EPR1", "E900",
				"EP28", "EP37")
				.forEach(code -> assertTrue("verification code " + code,
						cotisationsNonImposables.stream().filter(c -> c.getCode().equals(code)).findAny().isPresent()));

	}

	@Test
	public void test_cotisationImposables() {
		List<Cotisation> cotisationsImposables = bulletin1.getRemunerationEmploye().getProfilRemuneration()
				.getCotisationsImposables();
		Stream.of("SP01", "SP02")
				.forEach(code -> assertTrue("verification code " + code,
						cotisationsImposables.stream().filter(c -> c.getCode().equals(code)).findAny().isPresent()));

	}

	@Test
	public void test_grade() {
		assertThat(bulletin1.getRemunerationEmploye().getGrade().getNbHeuresBase(), equalTo(new BigDecimal("151.67")));
		assertThat(bulletin1.getRemunerationEmploye().getGrade().getTauxBase(), equalTo(new BigDecimal("11.0984")));
	}

	@After
	public void onExit() {
		context.close();
	}

}
```

* Créer un fichier _jdd-config.xml_ qui permet de rendre le test passant.

Quelques indications :


```xml
<!-- exemple de valorisation de liste -->
<bean id="profil1" class="dev.paie.entite.ProfilRemuneration">
    <property name="cotisationsNonImposables">
        <util:list value-type="dev.paie.entite.Cotisation">
            <ref bean="ep01" />
        </util:list>
    </property>
</bean>

<!-- importer une configuration dans une autre -->
<import resource="classpath:cotisations-non-imposables.xml" />
```

## AssertJ
* Vous vous souvenez d'_AssertJ_ ? Bien sûr que oui ! Remplacer les assertions des tests par des assertions _AssertJ_.
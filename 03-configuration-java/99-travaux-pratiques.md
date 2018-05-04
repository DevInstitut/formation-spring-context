# TP #2 - Configuration Java

= Formation Spring - Core (Java)
:source-highlighter: coderay
:title-logo-image: image:images/diginamic/diginamic_logo.png[pdfwitdh=100vw]
:author: (Auteur, Animateur) => 'Rossi Oddet'

## Partie 1 - Prise en main Spring Test

* Ajouter la dépendance vers _spring-test_ :

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-test</artifactId>
</dependency>
```

* Créer une classe _dev.paie.entite.ResultatCalculRemuneration_ :

```java
public class ResultatCalculRemuneration {
	
	private String salaireDeBase;
	private String salaireBrut;
	private String totalRetenueSalarial;
	private String totalCotisationsPatronales;
	private String netImposable;
	private String netAPayer;

	// TODO getters & setters à générer
}
```

* Créer une interface _dev.paie.service.CalculerRemunerationService_ :

```java
public interface CalculerRemunerationService {
	ResultatCalculRemuneration calculer(BulletinSalaire bulletin);
}
```

* Créer une implémentation _dev.paie.service.CalculerRemunerationServiceSimple_ :

```java
@Service
public class CalculerRemunerationServiceSimple implements CalculerRemunerationService {
	...
}
```

* Créer une classe _dev.paie.config.ServicesConfig_ de configuration pour les services de l'application :

```java
@Configuration
@ComponentScan("dev.paie.service")
public class ServicesConfig {

}
```

Cette configuration permet de rechercher des beans Spring dans le package _dev.paie.service_.

* Créer une classe de test _dev.paie.service.CalculerRemunerationServiceSimpleTest_ :

```java
// Sélection des classes de configuration Spring à utiliser lors du test
@ContextConfiguration(classes = { ServicesConfig.class })
// Configuration JUnit pour que Spring prenne la main sur le cycle de vie du test
@RunWith(SpringRunner.class)
public class CalculerRemunerationServiceSimpleTest {

	@Autowired private CalculerRemunerationService remunerationService;

	@Test
	public void test_calculer() {
		// TODO remplacer null par un objet bulletin
		ResultatCalculRemuneration resultat = remunerationService.calculer(null);
		assertThat(resultat.getSalaireBrut(), equalTo("2683.30"));
		assertThat(resultat.getTotalRetenueSalarial(), equalTo("517.08"));
		assertThat(resultat.getTotalCotisationsPatronales(), equalTo("1096.13"));
		assertThat(resultat.getNetImposable(), equalTo("2166.22"));
		assertThat(resultat.getNetAPayer(), equalTo("2088.41"));
	}
}
```

* Exécuter la classe de test. Vérifier que le service CalculerRemunerationService est bien injecté.

## Partie 2 - Implémentation CalculerRemunerationServiceSimple

* Compléter la classe de test _CalculerRemunerationServiceSimpleTest_ pour injecter un objet de type _BulletinSalaire_ provenant du jeux de données existant.
Utiliser cet objet lors de l'appel à la méthode _calculer_.

* Implémenter le service comme suit pour que le test soit passant :

```
SALAIRE_BASE = GRADE.NB_HEURES_BASE * GRADE.TAUX_BASE

SALAIRE_BRUT = SALAIRE_BASE + PRIME_EXCEPTIONNELLE

TOTAL_RETENUE_SALARIALE = SOMME(COTISATION_NON_IMPOSABLE.TAUX_SALARIAL*SALAIRE_BRUT)

TOTAL_COTISATIONS_PATRONALES = SOMME(COTISATION_NON_IMPOSABLE.TAUX_PATRONAL*SALAIRE_BRUT)

NET_IMPOSABLE = SALAIRE_BRUT - TOTAL_RETENUE_SALARIALE

NET_A_PAYER = NET_IMPOSABLE - SOMME(COTISATION_IMPOSABLE.TAUX_SALARIAL*SALAIRE_BRUT)
```

Indication:
* Exemple de récupération d'une configuration XML

```java
// Marque un bean de configuration Spring
@Configuration
// Import de la configuration XML dans une configuration Java
@ImportResource("classpath:jdd-config.xml")
public class JeuxDeDonneesConfig {

}
``` 

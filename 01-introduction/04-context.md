# Contexte Spring

Un contexte Spring peut être initialisé depuis n'importe quel environnement :

* Un test unitaire JUnit
* Une application web
* Un EJB
* Une application autonome (console, desktop, batch, ...)

Un contexte est initialisé à partir d'un fichier de configuration.

Un contexte est une implémentation de l’interface **org.springframework.context.ApplicationContext**.


Spring met à disposition de nombreuses implementations de l'interface **o.s.c.ApplicationContext** :

* **ClassPathXmlApplicationContext** : chargement du contexte depuis une source XML présente dans le classpath.
* **FileSystemXmlApplicationContext** : chargement du contexte depuis une source XML depuis le système de fichier.
* **XmlWebApplicationContext** : chargement du contexte depuis une source XML dans le contexte d’une application web.
* **AnnotationConfigApplicationContext** : chargement du contexte à partir de classes annotées.
* **AnnotationConfigWebApplicationContext** : chargement de contexte à partir de classes annotées au sein d’une application Web.
* ...
# Stéréotypes

Un stereotype permet de caractériser le rôle d'un bean au sein du contexte

Spring propose "en standard" quelques stéréotypes.

D'autres briques de Spring viennent compléter la liste de ces stereotypes.

[ditaa]
----
                +------------+
     +--------->+ @Component +<--------+
     |          +--------+---+         |
+----+----+     ^        ^     +-------+------+
|@Service |     |        |     |@Configuration|
+---------+     |        |     +--------------+
                |        |
      +---------+---+ +--+--------+
      | @Repository | |@Controller|
      +-------------+ +-----------+
----

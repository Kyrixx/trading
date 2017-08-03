# Utilisation du répertoire git

Sal's les noobz !

Voici un lien qui vous détaillera l'installation, l'initialisation d'un dossier GIT sur votre PC, et des principales commandes : https://openclassrooms.com/courses/gerez-vos-codes-source-avec-git

# Structure du code
Le langage utilisé est le PhP basé sur la POO (Programmation Orienté Objet). Cette approche me semble plus simple, et plus clair pour l'écriture d'algorithme tels qu'on le fait.
Je vais revenir sur certains aspects de la POO.

## Les classes
L'objectif de la POO, comme son nom l'indique, est de manipuler des objets. Par exemple, un stylo est un objet, possédants des paramètres (ou attributs en POO) comme la couleur d'écriture, la forme du stylo, et possède des fonctionnalités (ou méthodes en POO) comme sortir la mine, rentrer la mine.
Voici comme la classe Stylo s'écrirait en php :
```php
// On utilise le mot clé 'class' suivi du nom de la classe
class Stylo 
{
	private $couleurEncre;
	private $formeStylo;

	public function sortirMine()
	{

	}

	public function rentrerMine()
	{

	}
}
```
On a donc créé le canvas d'un stylo. Pour créer ces stylos et les stocker dans une variable pour les utiliser par la suite, on procède comme il suit :
```php
$monNouveauStylo = new Stylo;
$monNouveauStylo->sortirMine();
```
On a créé une instance de la classe Stylo.
L'avantage de travailler en POO est que ca nous permet de découper un problème complexe en plein de problème simple. Une fois qu'on a codé comment sortir une mine, il nous suffit juste d'appeller cette fonction, sans se soucier à chaque fois de ce qu'elle fait (pour autant qu'elle soit bien codée).
Cependant, notre stylo n'a pas encore de paramètre défini. Pour cela, nous allons utiliser le concept de Constructeur d'une classe :
```php
class Stylo 
{
	private $couleurEncre;
	private $formeStylo;

	public function __construct($couleur, $forme)
	{

	}

	public function sortirMine()
	{

	}

	public function rentrerMine()
	{

	}
}
```
Lors de l'instantiation d'un objet de la classe stylo, le constructeur sera automatiquement appellé.
```php
$monNouveauStyloBleuLong = new Stylo('bleu', 'long');
$monNouveauStyloBleuLong->sortirMine();
```
Si on veut faire appel à l'objet lui-même dans la classe Stylo, pour changer sa couleur par exemple,il faut utiliser la variable $this :
```php
class Stylo 
{
	private $couleurEncre;

	public function changerCouleur($nouvelleCouleur)
	{
		$this->couleurEncre = $nouvelleCouleur;
	}

	...
}
```
On peut donner des autorisations d'accès à certaines éléments de la classes, attributs ou méthodes, par les mots clés public, protected, private.
Lorsqu'un attribut (ou méthode) est publique, il est peut être modifié à l'extérieur de la classe (la méthode peut être appellé à l'extérieur de la classe), et inversement pour le mot clé private.
```php
class Stylo 
{
	private $couleurEncre;

	public function changerCouleur($nouvelleCouleur)
	{
		$this->couleurEncre = $nouvelleCouleur;
	}

	...
}

$monStylo = new Stylo;
$monStylo->couleurEncre = 'rouge'; // ERREUR : couleurEncre est privé
$monStylo->changerCouleur('rouge'); //CORRECT : changerCouleur est publique
```
Le mot-clé protected est utilisé pour l'héritage de classe.

## L'héritage de classe
Imaginons que nous avons plusieurs types de stylos : des stylos BICs, des crayons de papier, des stylos à encre etc. Nous savons que tous ces stylos ont un point commun, ce sont des stylos. On va donc reprendre notre classe Stylo :
```php
class Stylo 
{
	protected $couleurEncre; // On a remplacé private par protected
	protected $formeStylo; // On a remplacé private par protected

	public function __construct($couleur, $forme)
	{

	}

	public function sortirMine()
	{

	}

	public function rentrerMine()
	{

	}

	public function changerCouleur($nouvelleCouleur)
	{
		$this->couleurEncre = $nouvelleCouleur;
	}
}
```
Notre stylo à encre possède juste un attribut en plus qui est le niveau d'encre. On va donc créer une classe qui hérite de la classe Stylo et qui possède l'attribut de niveau d'encre en plus :
```php
class StyloEncre extends Stylo
{
	private $niveauEncre;

	public function __construct($couleur, $forme)
	{
		parent::_construct($couleur, $forme); //On appelle ici le constructeur de la classe Mère
	}

	public function getNiveauEncre()
	{
		return $this->niveauEncre;
	}

	public function changementCartouche($couleur)
	{
		$this->couleurEncre = $couleur; //On peut faire ca car $couleurEncre est un attribut protected, il peut être modifié dans les classes filles.
	}
}

$monStyloAEncre = new StyloEncre('bleu','long');
$monstyloAEncre->sortirMine(); //CORRECT : StyloEncre hérite de Stylo, donc on peut utiliser ses méthodes et attributs publics
$monstyloAEncre->couleurEncre = 'vert'; //ERREUR : un attribut protected ne peut pas être modifié en dehors de la classe ou il est défini, ou en dehors des classes filles.
```
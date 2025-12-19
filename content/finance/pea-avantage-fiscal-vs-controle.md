+++
title = "PEA: Avantage fiscal contre perte de contrôle"
date = 2025-11-20
description = "Les raisons pour lesquelles j'ai clôturé mon PEA prématurément pour passer sur CTO."
+++

J’ai acheté mes premières actions il y a quelques années. Et comme beaucoup de Français, j’ai commencé avec un **PEA** pour profiter des avantages fiscaux.

Je l’ai finalement fermé l’année dernière, après seulement deux ans, pour passer sur un **CTO**.

Le PEA a ses avantages, mais il comporte aussi des contraintes, qui peuvent coûter cher. Rien n’est gratuit.

J’explique mon raisonnement dans cet article. Je ne suis pas économiste, mais je ne penses pas être à côté de la plaque.

# Fonctionnement du PEA

Le Plan d’Épargne en Action (PEA) est un produit d’épargne français permettant d’investir dans des actions d’entreprises européennes ou des OPC (OPCVM, ETF, SICAV) qui respectent certains critères.

- Le plafond des versements sur un PEA classique est de 150 000 €.
- Ce plafond concerne les **versements**, pas la valorisation du compte. Autrement dit, la valeur du PEA peut dépasser ce plafond si les investissements génèrent des plus-values ou des dividendes.
- Retirer le moindre centime **avant 5 ans** entraîne normalement la **clôture** du PEA, et les gains sont imposés (30%).
- Une fois que le PEA a plus de 5 ans, les retraits partiels sont possibles sans fermer le PEA, et les gains ne sont plus soumis à l’impôt sur le revenu, mais restent soumis aux **prélèvements sociaux** (17,2 %)

Un des gros avantages en dehors des avantages fiscaux après 5 ans, c’est la possibilité de réinvestir les gains et dividendes sans passer par la case impôts. 

# Un avantage fiscal - En quel honneur ?

Alors que l’État français cherche à nous faire les poches, on peut se demander: pourquoi ce cadeau ?

Réponse: parce que le PEA est un outil de politique économique.

L’objectif est d’inciter les Français à investir **en Europe**, pas aux US.

Comme les entreprises américaines ont souvent des meilleurs rendements, il faut bien un petit bonus fiscal pour compenser et faire rester l’épargne en Europe.

Et comme notre cher président Emmanuel Macron aime l’Europe plus que tout, rien d’étonnant à ce que ce dispositif existe toujours.

Sans cet avantage fiscal, les français n’investiraient sans doute pas autant sur les marchés européens.

Sauf que… moi, comme beaucoup d’autres, j’achetais surtout du **MSCI World** sur mon PEA. Et le MSCI World, c’est environ 70% USA.

Comment ce type d’ETF peut être eligible au PEA ? 

# L’ETF MSCI World PEA

Tous les ETF MSCI World ne sont pas éligibles au PEA. Seuls quelques-uns le sont. Le plus connu, c’est **CW8** (Amundi), qui pèse plus de 5 milliards d’euros.

![ETFs MSCI World pour PEA](/pea-avantage-fiscal-vs-controle/etfs-pea.png)

Pour être éligible au PEA, un ETF doit contenir **au moins 75% d’actions éligibles au PEA**.

Comme le vrai MSCI World est composé majoritairement d’actions américaines, japonaises, etc.., ce n’est pas possible en réplication physique.

La seule solution: un ETF **synthétique**, via un contrat de swap.

L’ETF MSCI World que j’ai dans mon **CTO** (IE00B4L5Y983 de iShares) est en réplication physique. 

C’est à dire que Blackrock achète les vraies actions dans les bonnes proportions pour reproduire l’indice.

Un ETF MSCI World synthétique comme CW8 délivre la performance du MSCI World, sans être constitué des actions du MSCI World - puisque le PEA ne peut pas détenir celles ci.

Pour rentrer un peu plus dans les détails

- Amundi créé un panier d’actions (au moins 75% d’actions éligibles) qui délivre une performance proche du MSCI World.
- Un contrat de swap est mis en place entre Amundi et une banque (ici BNP Paribas). Amundi échange la performance de ce panier contre la performance du MSCI World.

On pense investir sur le MSCI World, mais l’argent est en réalité placé sur un panier d’actions européennes. La performance vient du swap.

Ça fonctionne. Mais ça ajoute des risques que n’a pas un ETF à replication physique.

# Quels sont les risques du synthétique ?

![Principales composantes du MSCI World](/pea-avantage-fiscal-vs-controle/actions-msci.png)
![Principales composantes de l'ETF synthétique](/pea-avantage-fiscal-vs-controle/actions-substitution.png)

Les tableaux parlent d’eux-mêmes: le panier de substitution n’a rien à voir avec le MSCI World. Mis à part NVIDIA que l’on retrouve des deux côtés, la base.

---

### **Remarque un peu hors sujet**

SIEMENS ENERGY AG représente 9,36 % du panier de substitution. C’est à dire environ 500 millions d’€.

Sans cet ETF synthétique, Siemens Energy n’aurait probablement pas reçu ces 500 millions.

Pour notre situation, ce n’est sans doute pas un problème. Mais si on imagine tous les ETF synthétiques, ça doit forcément foutre le bordel.

Bref, je m’égare. Peut être un sujet pour une prochaine fois.

---

## Risque de contrepartie

La performance n’est pas délivrée par les actions que détient l’ETF, mais par le contrat passé avec la BNP (la contrepartie). Le risque, c’est que BNP peut ne plus être en capacité de délivrer la performance de l’indice.

Dans ce cas on se retrouve avec la performance du panier de substitution. Et comme on l’a vu, ça n’a rien à voir avec le MSCI World.

Je pense qu’en temps normal, ça ne pose aucun problème.

- BNP est très solide.
- Le panier qui sert de collatéral est censé avoir une performance proche de l’indice.

Mais si BNP saute, c’est probablement dans un contexte de crise majeure. Et dans ce cas, on se retrouvera avec de l’industrie Allemande à la place du MSCI World dans le PEA.

Mieux que rien… mais absolument pas ce que l’on avait acheté.

# Manque de flexibilité

Quand j’ai clôturé mon PEA, j’ai mis **1 mois** pour récupérer mes fonds. Pourtant j’étais chez Fortuneo, loin d’être la pire banque.

C’est très long.

Sachant que le choix de ce que l’on peut acheter est limité, et que l’on ne peut rester qu’en euros à l’intérieur, ça me dérange d’avoir une grosse partie de mon patrimoine bloqué dans le PEA. 

Si on doit réagir vite, c’est compliqué.

# Conclusion

Il y a de très belles actions éligibles au PEA, qui n’ont rien à envier aux actions US: LVMH, Air Liquide, Schneider, Total…

Et avec des ETF comme CW8, on peut s’exposer au MSCI World tout en profitant d’une fiscalité avantageuse.

Mais il y a un cout à payer pour tout ça: le risque de contrepartie des ETF synthétiques, et le manque de flexibilité. 

Dans un contexte normal, je ne pense pas que ça pose problème.

Mais le jour où ça déraille, il y a un fort risque de perdre très gros.

Au vu de la situation économique d’aujourd’hui, et encore plus après avoir lu “how countries go broke” de Ray Dalio, je préfère éviter de m’exposer à ces risques.

En étant sur CTO, je suis libre de vendre et d’acheter absolument tout:
- Soit directement sur le CTO
- Soit en me faisant un virement instantané sur Revolut ou Binance par exemple, pour avoir accès à d'autres produits.

Je peux rapidement me positionner en Actions, ETF, Or, Crypto, devises, .. Et donc être en capacité de protéger mon épargne dans toutes les situations.

C’est le choix que j’ai fait. Ça ne veut pas dire que c’est le meilleur choix, mais c’est celui qui me convient pour le moment.

Mieux vaut gagner un peu moins que tout perdre.

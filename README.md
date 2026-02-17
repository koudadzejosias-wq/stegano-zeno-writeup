Titre : Dumpster Diving, Wordlists et stéganographie avec steghide/stegseek

Contexte

Catégorie : Stéganographie
Objectif : Extraire un drapeau caché dans une image JPEG protégée par mot de passe.
Indice fourni : Les mots de passe du Dr. Zeno font entre 6 et 7 caractères et n'utilisent que les symboles de l'alphabet suivant : ezdot-g.4#$18_593
Outils utilisés

file: identifiant le type réel d'un fichier
exiftool : analyseur les métadonnées d'images
steghide : outil de stéganographie (insertion/extraction)
stegseek: attaque par dictionnaire optimisé pour steghide
crunch: génération de liste de mots personnalisée
Résumé de l'approche

Décompression de l'archive zip pour récupérer l'image.
Vérifications : type de fichier et présence éventuelle de données cachées.
Génération d'une wordlist contrainte par l'alphabet et la longueur (6–7).
Attaque par dictionnaire avec stegseek contre steghide.
Lecture du fichier extrait contenant le drapeau.
Étapes détaillées

Extraction de l'archive
frapper
unzip "Dr. Zeno.zip"
ls -la
On vérifie que le JPG a bien été extrait (par exemple Dr. Zeno.jpg).
Vérifications et détection de stéganographie
Confirmer le type réel du fichier (éviter les leurres d'extension).
frapper
file "Dr. Zeno.jpg"
exiftool "Dr. Zeno.jpg"
steghide info "Dr. Zeno.jpg"
Résultat attendu :

le fichier indique un « JPEG image data » valide.
exiftool : aucune métadonnée suspecte particulière (facultatif).
steghide info: indique la présence de données cachées et précise qu'elles sont protégées par mot de passe.
Génération de la wordlist avec crunch
Alphabet : ezdot-g.4#$18_593 (17 caractères)
Longueur : 6 à 7
Important : placer l'alphabet entre guillemets pour que le shell n'interprète pas les caractères spéciaux (#, $).
frapper
crunch 6 7 'ezdot-g.4#$18_593' -o zeno.txt
Remarque :

Espace de recherche théorique : 17^6 + 17^7 ≈ 4,34×10^8 combinaisons. C'est donc, mais stegseek est très optimisé et ne parcourt généralement pas naïvement toute la liste.
Attaque dictionnaire avec stegseek
stegseek détecte automatiquement le format steghide, teste les mots de passe depuis la wordlist et extrait le contenu si succès.
frapper
stegseek "Dr. Zeno.jpg" zeno.txt
Résultat attendu :

Affichage d'un « Mot de passe trouvé » avec le mot de passe correspondant.
Extraction automatique d'un fichier « Dr. Zeno.jpg.out ».
Astuce :

Si vous souhaitez réutiliser steghide après avoir trouvé le mot de passe : steghide extract -sf "Dr. Zeno.jpg" -p "LE_MOT_DE_PASSE"
Mais stegseek gère déjà l'extraction automatiquement.
Lecture du drapeau
frapper
cat "Dr. Zeno.jpg.out"
Drapeau : IPNET{dUmps73r_d1v1nG_&&_w0rdl1sT_c4N_H3lp_y0u_t0_r3tri3v3_p4ssssw0r$$D}

Pourquoi cette méthode fonctionne

Dumpster Diving: l'énoncé suggère un mot de passe faible, court et avec un alphabet limité.
Réduction de l'espace de recherche : en contraignant la longueur (6–7) et l'alphabet (17 symboles), la liste de mots devient attaquable.
stegseek : spécialement optimisé pour casser les passphrases steghide bien plus vite qu'une simple itération naïve.
Bonnes pratiques et pièges à éviter

Quoter l'alphabet dans crunch : utilisez des quotes pour éviter que $ et # ne soient interprétés par le shell.
Vérifiez toujours le type réel du fichier (file) pour éviter les faux positifs.
Si la liste de mots devient trop lourde :
Prévoir de la place disque et du temps CPU.
Segmenter la liste de mots (split) si nécessaire.
Alternatives :
Utiliser des listes de mots thématiques si d'autres indices existants.
Testeur d'abord la longueur 6 seule, puis 7 (si contraintes de temps/espace).
Performance:
Éviter d'exécuter d'autres tâches gourmandes en CPU en parallèle.
Utilisez un SSD pour les E/S si la liste de mots est volumineuse.
Reproduction rapide (aide-mémoire)

frapper
unzip "Dr. Zeno.zip"
file "Dr. Zeno.jpg"
steghide info "Dr. Zeno.jpg"
crunch 6 7 'ezdot-g.4#$18_593' -o zeno.txt
stegseek "Dr. Zeno.jpg" zeno.txt
cat "Dr. Zeno.jpg.out"
Conclusion En exploitant l'indice sur la politique de mots de passe du Dr Zeno, nous avons réduit efficacement l'espace de recherche et utilisé une chaîne d'outils parfaitement adaptée (crunch → stegseek/steghide) pour extraire le contenu caché. Le drapeau est récupéré avec une approche simple, reproductible et rationnelle.



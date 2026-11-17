# Modelization
pour couzin for birds :

**1. Prédateur (faucon)**
- Bouton *Ajouter / Retirer un faucon*, et curseur de vitesse du faucon.
- Le faucon poursuit l'oiseau vivant le plus proche. Quand il capture un oiseau, celui-ci disparaît (plumes en particules), et le faucon reste 3 s en « repos » avant de rechasser.
- Les oiseaux ne fuient que s'ils **perçoivent** le faucon (dans leur champ de perception $\alpha$, donc l'angle mort arrière reste actif). Les autres suivent leurs voisins : c'est la transmission d'information décrite dans la Discussion de l'article.

**2. Obstacles**
- Bouton *Ajouter un obstacle* : pylône ou rocher, placé devant la nuée dans sa direction de vol.
- Bouton *Retirer les obstacles ajoutés*.
- Les obstacles de décor (troncs, rochers) sont aussi des obstacles pour les oiseaux.

**3. Environnements** (sélecteur en haut du panneau droit)
- **Aucun** : ciel vide, conforme au modèle de l'article.
- **Forêt de collines** : relief fbm, 300 pins dont les troncs sont des obstacles.
- **Lac & rives** : cuvette remplie d'eau, arbres sur les rives.
- **Montagnes & rochers** : crêtes enneigées, blocs rocheux comme obstacles.
- **Plaine & acacias** : relief doux, acacias.

### Formules ajoutées

Ces règles ne figurent pas dans l'article. Elles s'ajoutent au-dessus des règles de Couzin, avec une priorité décroissante : **obstacles et relief > fuite > zone de répulsion > zoo/zoa**.

- **Distance à un obstacle** (capsule verticale de rayon $R_k$, axe $[\mathbf{A}_k,\mathbf{B}_k]$) : $d_k = |\mathbf{c}_i - \mathbf{q}_k| - R_k$, où $\mathbf{q}_k$ est le point de l'axe le plus proche de $\mathbf{c}_i$.
- **Évitement d'obstacle** (si $d_k < R_{obs}$ et obstacle visible, dans le cône ou à distance $< r_r$) : $\mathbf{d}_{obs} = \sum_k w_k \,\frac{\mathbf{c}_i - \mathbf{q}_k}{|\mathbf{c}_i - \mathbf{q}_k|}$, avec $w_k = \frac{R_{obs} - d_k}{R_{obs}}$.
- **Relief** : $g_i = y_i - h(x_i, z_i)$. Si $g_i < R_{sol}$, on ajoute $w = \frac{R_{sol} - g_i}{R_{sol}}$ fois la normale au terrain.
- **Fuite** (faucon $\mathbf{p}$ perçu à $d < R_{fear}$ dans le cône) : $\mathbf{d}_f = -\frac{\mathbf{p} - \mathbf{c}_i}{|\mathbf{p} - \mathbf{c}_i|}$.
- **Urgence** (fuite ou évitement) : la rotation maximale devient $2{,}5\,\theta\tau$. Le bruit $\sigma$ reste appliqué.
- **Contrainte dure** : aucun oiseau ne peut pénétrer un obstacle ni passer sous le sol.
- **Capture** : si $|\mathbf{p} - \mathbf{c}_i| < 1{,}4$.

Les métriques $p_{group}$, $m_{group}$ et le plan frontal ne portent que sur les oiseaux vivants.
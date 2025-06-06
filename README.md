<h1>🗂️ Atelier – Sauvegarde, Restauration et Supervision dans MongoDB</h1>

<p>🎯 <strong>Objectif</strong><br>
Cet atelier vous guide à travers les opérations essentielles de sauvegarde, restauration et supervision d'une base de données MongoDB, en utilisant MongoDB Atlas et ses outils associés.</p>

<h2>🧠 Ce que vous apprendrez</h2>
<ul>
  <li>Configurer un cluster MongoDB Atlas</li>
  <li>Administrer une base de données</li>
  <li>Exporter / importer des données</li>
  <li>Sauvegarder et restaurer des bases</li>
  <li>Superviser les performances</li>
  <li>Analyser les logs</li>
</ul>

<h2>👥 Réalisé&nbsp;par</h2>
<ul>
  <li>Essadikine Anass</li>
  <li>Elmostafa Chtiba</li>
  <li>Belaatik Nizar</li>
  <li>Belhaous Abdeladim</li>
  <li>Taki Youssef</li>
  <li>Sallama Abdessamad</li>
  <li>Baquouch Amine</li>
</ul>

<div class="warning">
  <h3>⚠️ Important</h3>
  <p>Pour les utilisateurs disposant déjà d’un compte MongoDB Atlas, il est recommandé de créer un <strong>nouveau compte dédié</strong> à cet atelier afin d’éviter toute confusion avec vos projets existants.</p>
</div>

<h2>Prérequis</h2>
<ul>
  <li>Un compte MongoDB Atlas</li>
  <li>MongoDB Compass</li>
  <li>Outils en ligne de commande MongoDB (<code>mongodump</code>, <code>mongoexport</code>, etc.)</li>
</ul>

<h2>🧩 Modules de l’Atelier</h2>

<h3>📁 Module&nbsp;1 : Configuration Initiale</h3>
<ol>
  <li>Créer un compte <a href="https://www.mongodb.com/atlas/database" target="_blank">MongoDB Atlas</a></li>
  <li>Créer un cluster gratuit :
    <ul>
      <li>Sélectionnez le tier&nbsp;<strong>M0 (gratuit)</strong></li>
      <li>Choisissez une région proche de vous</li>
    </ul>
  </li>
  <li>Charger le jeu de données <code>sample_airbnb</code> :
    <ol>
      <li>Dans l’interface Atlas, cliquez sur <strong>…</strong> à côté de votre cluster</li>
      <li>Sélectionnez <strong>Load Sample Dataset</strong></li>
    </ol>
  </li>
  <li>Configurer la sécurité :
    <ul>
      <li><strong>Créer un utilisateur</strong> :
        <ol>
          <li>Menu <strong>Database Access</strong> ▶ <strong>Add New Database User</strong></li>
          <li>Choisissez <em>Password Authentication</em>, puis définissez un utilisateur/mot de passe</li>
          <li>Attribuez le rôle <strong>Atlas Admin</strong></li>
        </ol>
      </li>
      <li><strong>Autoriser les accès réseau</strong> :
        <ol>
          <li>Menu <strong>Network Access</strong> ▶ <strong>Add IP Address</strong></li>
          <li>Indiquez <code>0.0.0.0/0</code> (pour l’atelier uniquement)</li>
        </ol>
      </li>
    </ul>
  </li>
</ol>

<h3>🔍 Module&nbsp;2 : Exploration des Données</h3>
<ol>
  <li>Se connecter avec MongoDB Compass :
    <ul>
      <li>Récupérez l’<abbr title="Uniform Resource Identifier">URI</abbr> de connexion (<em>Connect › Compass</em>)</li>
      <li>Collez l’URI dans Compass et connectez-vous</li>
    </ul>
  </li>
  <li>Explorer la collection <code>listingsAndReviews</code> :
    <ul>
      <li>Base : <code>sample_airbnb</code></li>
      <li>Onglet <strong>Schema</strong> pour visualiser la structure</li>
    </ul>
  </li>
  <li>Requêtes d’exemple :</li>
</ol>

<pre><code class="language-json">// Appartements avec &gt; 3 chambres
{ "bedrooms": { "$gt": 3 } }

// Appartements à New York
{ "address.market": "New York" }

// Requête illogique (aucun résultat)
{ "price": -1 }
</code></pre>

<h3>📤 Module&nbsp;3 : Import / Export de Données</h3>

<p>Téléchargez les <a href="https://www.mongodb.com/try/download/database-tools" target="_blank">MongoDB Command Line Database Tools</a>.</p>

<p><strong>Exporter</strong> une collection avec <code>mongoexport</code> :</p>
<pre><code class="language-bash">mongoexport --uri="mongodb+srv://&lt;username&gt;:&lt;password&gt;@cluster0.abc123.mongodb.net/sample_airbnb" \
  --collection=listingsAndReviews --out=listings.json
</code></pre>

<p><strong>Importer</strong> des données avec <code>mongoimport</code> :</p>
<pre><code class="language-bash">mongoimport --uri="mongodb+srv://&lt;username&gt;:&lt;password&gt;@cluster0.abc123.mongodb.net/sample_airbnb" \
  --collection=listingsAndReviews --file=listings.json
</code></pre>

<h3>💾 Module&nbsp;4 : Sauvegarde et Restauration</h3>

<p><strong>Sauvegarder</strong> la base avec <code>mongodump</code> :</p>
<pre><code class="language-bash">mongodump --uri="mongodb+srv://&lt;username&gt;:&lt;password&gt;@cluster0.abc123.mongodb.net/sample_airbnb" \
  --out=sauvegarde_airbnb
</code></pre>

<p>Version archive + compression :</p>
<pre><code class="language-bash">mongodump --uri="mongodb+srv://&lt;username&gt;:&lt;password&gt;@cluster0.abc123.mongodb.net/sample_airbnb" \
  --archive=backup_airbnb.archive --gzip
</code></pre>

<p><strong>Restaurer</strong> dans une nouvelle base avec <code>mongorestore</code> :</p>
<pre><code class="language-bash">mongorestore --uri="mongodb+srv://&lt;username&gt;:&lt;password&gt;@cluster0.abc123.mongodb.net" \
  --nsFrom="sample_airbnb.*" --nsTo="restored_airbnb.*" \
  --archive=backup_airbnb.archive --gzip --drop
</code></pre>

<h3>📊 Module&nbsp;5 : Supervision</h3>
<ol>
  <li>Dans Atlas, ouvrez l’onglet <strong>Metrics</strong> de votre cluster</li>
  <li>Surveillez :
    <ul>
      <li>Connections</li>
      <li>Network</li>
      <li>Logical Size</li>
      <li>Opcounters</li>
    </ul>
  </li>
  <li>Configurer des alertes :
    <ol>
      <li>Menu <strong>Alerts</strong> ▶ <strong>Create Alert</strong></li>
      <li>Choisissez une catégorie (ex. <em>Host</em>), une condition (ex. <em>Host is down</em>)</li>
      <li>Cochez <em>Email</em>, puis <strong>Save</strong></li>
    </ol>
  </li>
</ol>

<h3>📝 Module&nbsp;6 : Gestion des Logs</h3>
<ol>
  <li>Menu <strong>Activity Feed</strong> de votre projet Atlas</li>
  <li>Filtrez par action, cluster ou période pour voir lectures, écritures, suppressions…</li>
</ol>

<div class="results">
  <h2>✅ Résultats Finaux</h2>
  <ul>
    <li>Déploiement d’un cluster Atlas configuré et sécurisé</li>
    <li>Exploration du dataset <code>sample_airbnb</code> via Compass</li>
    <li>Maîtrise des commandes <code>mongoexport</code>, <code>mongoimport</code>, <code>mongodump</code>, <code>mongorestore</code></li>
    <li>Supervision des métriques clés et mise en place d’alertes</li>
    <li>Analyse de l’Activity Feed pour la conformité et la performance</li>
  </ul>
</div>

<div class="deliverables">
  <h2>📎 Livrables</h2>
  <ul>
    <li>MongoDB Compass affichant les bases <code>restored_airbnb</code>, <code>sample_airbnb</code> et <code>test_airbnb</code></li>
    <li>Console montrant <code>mongoexport</code>, <code>mongodump</code> et <code>mongorestore</code> (5 555 documents)</li>
    <li>Dashboard Atlas : courbes <em>Opcounters</em>, <em>Logical Size</em>, <em>Connections</em>, <em>Network</em></li>
    <li>Écran Atlas illustrant la configuration des alertes</li>
    <li>Activity Feed listant les opérations réalisées</li>
  </ul>
</div>

<h2>📚 Ressources Utiles</h2>
<ul>
  <li><a href="https://www.mongodb.com/docs/atlas/" target="_blank">MongoDB Atlas Documentation</a></li>
  <li><a href="https://www.mongodb.com/try/download/compass" target="_blank">MongoDB Compass</a></li>
  <li><a href="https://www.mongodb.com/try/download/database-tools" target="_blank">MongoDB Tools</a></li>
</ul>

</body>
</html>

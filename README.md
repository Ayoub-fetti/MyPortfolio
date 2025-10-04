# Portfolio Vue.js

Portfolio personnel moderne et minimaliste construit avec Vue.js 3 et Tailwind CSS 4.

## 🚀 Fonctionnalités

- ✨ Design minimaliste et élégant
- 📱 Entièrement responsive
- 🎨 Thème personnalisable via Tailwind CSS
- 📂 Section projets avec liens GitHub et démo
- 💼 Section expérience professionnelle
- 🛠️ Section compétences techniques
- 🎓 Section formation
- 📄 Téléchargement de CV
- ⚡ Performance optimale avec Vite

## 📋 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- [Node.js](https://nodejs.org/) (version 18 ou supérieure)
- [npm](https://www.npmjs.com/) ou [yarn](https://yarnpkg.com/)

## 🔧 Installation

1. **Cloner le repository**

\`\`\`bash
git clone https://github.com/votre-username/portfolio-vuejs.git
cd portfolio-vuejs
\`\`\`

2. **Installer les dépendances**

\`\`\`bash
npm install
# ou
yarn install
\`\`\`

## 🏃 Lancer le projet localement

### Mode développement

\`\`\`bash
npm run dev
# ou
yarn dev
\`\`\`

Le site sera accessible à l'adresse : `http://localhost:5173`

### Build de production

\`\`\`bash
npm run build
# ou
yarn build
\`\`\`

Les fichiers optimisés seront générés dans le dossier `dist/`

### Prévisualiser le build de production

\`\`\`bash
npm run preview
# ou
yarn preview
\`\`\`

## 🎨 Personnalisation

### Informations personnelles

Modifiez les données dans `src/App.vue` :

- **Informations de base** : `name`, `title`, `bio`
- **Projets** : tableau `projects`
- **Expériences** : tableau `experiences`
- **Compétences** : tableau `skills`
- **Formation** : tableau `education`
- **Réseaux sociaux** : tableau `socials`

### Thème et couleurs

Les couleurs sont définies dans `src/assets/main.css` :

\`\`\`css
--color-background: #fafafa;
--color-foreground: #171717;
--color-muted: #f5f5f5;
--color-muted-foreground: #737373;
--color-accent: #e5e5e5;
--color-border: #e5e5e5;
--color-primary: #171717;
--color-primary-foreground: #fafafa;
\`\`\`

### Ajouter votre CV

1. Placez votre fichier CV (format PDF) dans le dossier `public/`
2. Nommez-le `cv.pdf` ou modifiez le chemin dans la fonction `downloadCV()` dans `src/App.vue`

## 📁 Structure du projet

\`\`\`
portfolio-vuejs/
├── public/
│   └── cv.pdf              # Votre CV en PDF
├── src/
│   ├── assets/
│   │   └── main.css        # Styles globaux et thème
│   ├── App.vue             # Composant principal
│   └── main.js             # Point d'entrée
├── index.html              # Template HTML
├── package.json            # Dépendances
├── vite.config.js          # Configuration Vite
└── README.md               # Documentation
\`\`\`

## 🛠️ Technologies utilisées

- **Vue.js 3** - Framework JavaScript progressif
- **Tailwind CSS 4** - Framework CSS utility-first
- **Vite** - Build tool ultra-rapide
- **JavaScript ES6+** - Langage de programmation

## 📱 Responsive Design

Le portfolio est entièrement responsive et s'adapte à tous les écrans :

- 📱 Mobile (< 768px)
- 💻 Tablette (768px - 1024px)
- 🖥️ Desktop (> 1024px)

## 🚀 Déploiement

### Vercel

\`\`\`bash
npm install -g vercel
vercel
\`\`\`

### Netlify

\`\`\`bash
npm run build
# Glissez-déposez le dossier dist/ sur Netlify
\`\`\`

### GitHub Pages

1. Modifiez `vite.config.js` pour ajouter la base :

\`\`\`js
export default defineConfig({
  base: '/nom-du-repo/',
  // ...
})
\`\`\`

2. Déployez :

\`\`\`bash
npm run build
git add dist -f
git commit -m "Deploy"
git subtree push --prefix dist origin gh-pages
\`\`\`

## 📝 Licence

Ce projet est sous licence MIT. Vous êtes libre de l'utiliser et de le modifier.

## 👤 Auteur

**Votre Nom**

- GitHub: [@votre-username](https://github.com/votre-username)
- LinkedIn: [Votre Nom](https://linkedin.com/in/votre-username)
- Email: votre.email@example.com

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à ouvrir une issue ou une pull request.

## 💡 Support

Si vous avez des questions ou besoin d'aide, n'hésitez pas à ouvrir une issue sur GitHub.

---

Fait avec ❤️ et Vue.js

# Atelier : Créer une application simple de liste de tâches avec Firebase et React

Dans cet atelier, nous allons créer une application simple de liste de tâches en utilisant Firebase pour la gestion des données et React pour la partie frontale de notre application.

## Partie 0 : Préparation de l'environnement

- Installez Node.js et npm : https://nodejs.org/en/download/
- Créez un nouveau projet Firebase : https://console.firebase.google.com/
- Installez Firebase CLI : `npm install -g firebase-tools`
- Connectez-vous à Firebase : `firebase login`
- Installez les dépendances du projet : `npm install`

## Partie 1 : Configuration de Firebase

1.1 Allez à la console Firebase, sélectionnez votre projet, puis cliquez sur le bouton "Ajouter une application" et suivez les instructions pour créer une nouvelle application Web.

1.2 Copiez les paramètres de configuration de votre application et collez-les dans un nouveau fichier `firebase.js` dans le dossier `src` de votre application React. Votre fichier `firebase.js` devrait ressembler à ceci :

```javascript
import firebase from 'firebase/app';
import 'firebase/firestore';

const firebaseConfig = {
  // Vos paramètres de configuration
};

firebase.initializeApp(firebaseConfig);

export const db = firebase.firestore();
```

## Partie 2 : Création du composant Todo

2.1 Créez un nouveau fichier `Todo.js` dans le dossier `src` et importez React, useState, useEffect et le module Firestore :

```javascript
import React, { useState, useEffect } from 'react';
import { db } from './firebase';
```

2.2 Dans le même fichier, créez un composant fonctionnel Todo et définissez un état pour stocker la liste des tâches :

```javascript
const Todo = () => {
  const [todos, setTodos] = useState([]);

  // ...
};

export default Todo;
```

2.3 Ajoutez une fonction `getTodos` pour récupérer la liste des tâches depuis Firestore et appelez cette fonction quand le composant est monté :

```javascript
const Todo = () => {
  // ...

  const getTodos = async () => {
    const todosCollection = await db.collection('todos').get();
    setTodos(todosCollection.docs.map(doc => doc.data()));
  };

  useEffect(() => {
    getTodos();
  }, []);

  // ...
};
```

2.4 Ajoutez une fonction `addTodo` pour ajouter une nouvelle tâche à Firestore et une fonction `deleteTodo` pour supprimer une tâche :

```javascript
const Todo = () => {
  // ...

  const addTodo = async (todo) => {
    await db.collection('todos').add(todo);
    getTodos();
  };

  const deleteTodo = async (id) => {
    await db.collection('todos').doc(id).delete();
    getTodos();
  };

  // ...
};
```

## Partie 3 : Ajout de l'interface utilisateur

3.1 Dans le composant Todo, ajoutez un formulaire pour ajouter une nouvelle tâche :

```javascript
// ...
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input type="text" value={todo} onChange={e => setTodo(e.target.value)} />
        <button type="submit">Add</button>
      </form>
      // ...
    </div>
  );
// ...
```

3.2 Ajoutez une liste pour afficher les tâ

ches :

```javascript
// ...
  return (
    <div>
      // ...
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo.text}
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
// ...
```

Bravo ! Vous avez maintenant une application de liste de tâches fonctionnelle qui utilise Firebase et React. Vous pouvez explorer davantage et ajouter d'autres fonctionnalités à votre application. Bon codage !

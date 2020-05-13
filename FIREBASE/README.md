# Firebase



## Geting doc

```js
db.collection("profile").doc("followers").get().then((doc) => {
          if (doc.exists) {
             this.setState({ user: doc.data() })
          } else {
            console.log("No such document!");
          }
        })
```



## Getting collection

```js
db.collection("cities").where("capital", "==", true).get().then((snapshot) => {
        snapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            console.log(doc.id, " => ", doc.data());
        });
    })
    .catch((error) => {
        console.log("Error getting documents: ", error);
    });
```

  **Usage in react**

```js
db.collection('posts').orderBy('lastModified', 'desc').get().then((snapshot) => {
            let posts = [];
            snapshot.forEach(doc => {
                const items = doc.data();
                posts.push(items);
            });
            this.setState({ posts });
       });
```



## Batches

```js
const batch = db.batch();

    const DeleteInviteRef = usersRef.doc(this.props.currentUser.username).collection('invites').doc(inviteId);
    batch.delete(DeleteInviteRef);

    const addAsMemeberRef = papersRef.doc(paperName);
    batch.update(addAsMemeberRef, { allMembers: firebase.firestore.FieldValue.arrayUnion(this.props.currentUser.username) });

    const addPaperToProfileRef = usersRef.doc(this.props.currentUser.username);
    batch.update(addPaperToProfileRef, { allPapers: firebase.firestore.FieldValue.arrayUnion(paperName), invites: firebase.firestore.FieldValue.increment(-1) });

    batch.commit().then(() => {
      //do anything
      });

    });
```



## Add timestamp to firestore

firebase.firestore.FieldValue.serverTimestamp()

It will get saved as timestamp.

**To use it**, first convert it to date format like database.createdOn.toDate()

And then use date-fns or whatever to format it 



## Multiple Firestore queries, single promise / callback

```js
user.tags = ["technology","politics","sports","architecture","business"];

var dbPromises = [];
for (var i = 0; i < user.tags.length; i++) {
  dbPromises.push(
      db.collection('articles')
        .where(user.tags[i], '>', 0)
        .orderBy(user.tags[i])
        .get()
  );
}

Promise.all(dbPromises)
    .then(() => {
        // ... push to article array
};
```

code not tested, just the idea on how to do it





## Adding Data

To create or overwrite a single document, use the `set()` method:

```js
// Add a new document in collection "cities"
db.collection("cities").doc("LA").set({
    name: "Los Angeles",
    state: "CA",
    country: "USA"
})
.then(function() {
    console.log("Document successfully written!");
})
.catch(function(error) {
    console.error("Error writing document: ", error);
});
```



If you don't have an doc id and want firestore to generate it for you, can can use add instead of set

```js
// Add a new document in collection "cities"
db.collection("cities").add({
    name: "Los Angeles",
    state: "CA",
    country: "USA"
})
.then(function(docRef) {
    console.log("Document written with ID: ", docRef.id);
})
.catch(function(error) {
    console.error("Error adding document: ", error);
});
```



In some cases, it can be useful to create a document reference with an auto-generated ID, then use the reference later. For this use case, you can call `doc()`:

```js
// Add a new document with a generated id.
var newCityRef = db.collection("cities").doc();

// later...
newCityRef.set(data);
```





## Difference between update and merge true

- `set` without merge will overwrite a document or create it if it doesn't exist yet
- `set` with merge will update fields in the document or create it if it doesn't exists
- `update` will update fields but will fail if the document doesn't exist



```js
batch.set(addToPaper, {
	latestPost: {
		"sharedByName": this.props.currentUser.name,
		"type": "share"
	}
}, {merge: true});
```



## Realtime Updates 



```js
componentDidMount() {
       this.unsubscribe = adminRef.collection('files').onSnapshot((snapshot) => {
           let files = [];
           snapshot.forEach(doc => {
               const items = doc.data();
               files.push(items);
            });
           this.setState({ files });
       });
     }
     
     componentWillUnmount() {
        this.unsubscribe();
     }
```


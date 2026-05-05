# 📱 Lab 3 – Programmation Mobile Android (Java)

## 🎯 Objectif

Dans ce lab, nous avons développé une application Android composée de deux écrans :

* 📝 **Écran 1 : Formulaire** (saisie des informations utilisateur)
* 📄 **Écran 2 : Récapitulatif** (affichage des données saisies)

👉 Les données sont envoyées entre les deux écrans via un **Intent explicite**.

---

## ⚙️ Pré-requis

* Android Studio (version récente)
* Projet **Empty Activity (Java)**
* Min SDK : 24+

---

## 🧩 Étape 1 : Interface du Formulaire

📁 `res/layout/activity_main.xml`

```xml
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="20dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView android:text="Nom & Prénom" android:textStyle="bold"/>
        <EditText
            android:id="@+id/nom"
            android:hint="Ex : Nom"
            android:inputType="textPersonName"/>

        <TextView android:text="Email" android:textStyle="bold"/>
        <EditText
            android:id="@+id/email"
            android:hint="Ex : user@gmail.com"
            android:inputType="textEmailAddress"/>

        <TextView android:text="Téléphone" android:textStyle="bold"/>
        <EditText
            android:id="@+id/phone"
            android:inputType="phone"/>

        <TextView android:text="Adresse" android:textStyle="bold"/>
        <EditText
            android:id="@+id/adresse"
            android:inputType="textPostalAddress"/>

        <TextView android:text="Ville" android:textStyle="bold"/>
        <EditText
            android:id="@+id/ville"/>

        <Button
            android:id="@+id/btnEnvoyer"
            android:text="Envoyer"/>
    </LinearLayout>
</ScrollView>
```

---

## 🧩 Étape 2 : Interface du Récapitulatif

📁 `res/layout/activity_screen2.xml`

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp">

    <TextView
        android:text="Screen2"
        android:textSize="22sp"
        android:textStyle="bold"/>

    <TextView
        android:id="@+id/recap"
        android:textSize="16sp"/>

    <Button
        android:id="@+id/btnRetour"
        android:text="Retour"/>
</LinearLayout>
```

---

## 💻 Étape 3 : Code du Formulaire

📁 `MainActivity.java`

```java
public class MainActivity extends AppCompatActivity {

    private EditText nom, email, phone, adresse, ville;
    private Button btnEnvoyer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        nom = findViewById(R.id.nom);
        email = findViewById(R.id.email);
        phone = findViewById(R.id.phone);
        adresse = findViewById(R.id.adresse);
        ville = findViewById(R.id.ville);
        btnEnvoyer = findViewById(R.id.btnEnvoyer);

        btnEnvoyer.setOnClickListener(v -> {

            String sNom = nom.getText().toString().trim();
            String sEmail = email.getText().toString().trim();
            String sPhone = phone.getText().toString().trim();
            String sAdresse = adresse.getText().toString().trim();
            String sVille = ville.getText().toString().trim();

            if (sNom.isEmpty() || sEmail.isEmpty()) {
                Toast.makeText(this, "Nom et Email obligatoires", Toast.LENGTH_SHORT).show();
                return;
            }

            Intent i = new Intent(MainActivity.this, Screen2Activity.class);

            i.putExtra("nom", sNom);
            i.putExtra("email", sEmail);
            i.putExtra("phone", sPhone);
            i.putExtra("adresse", sAdresse);
            i.putExtra("ville", sVille);

            startActivity(i);
        });
    }
}
```

---

## 💻 Étape 4 : Code du Récapitulatif

📁 `Screen2Activity.java`

```java
public class Screen2Activity extends AppCompatActivity {

    private TextView recap;
    private Button btnRetour;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_screen2);

        recap = findViewById(R.id.recap);
        btnRetour = findViewById(R.id.btnRetour);

        Intent intent = getIntent();

        String nom = intent.getStringExtra("nom");
        String email = intent.getStringExtra("email");
        String phone = intent.getStringExtra("phone");
        String adresse = intent.getStringExtra("adresse");
        String ville = intent.getStringExtra("ville");

        String resume =
                "Nom : " + safe(nom) +
                "\nEmail : " + safe(email) +
                "\nPhone : " + safe(phone) +
                "\nAdresse : " + safe(adresse) +
                "\nVille : " + safe(ville);

        recap.setText(resume);

        btnRetour.setOnClickListener(v -> finish());
    }

    private String safe(String s) {
        return (s == null || s.trim().isEmpty()) ? "-" : s;
    }
}
```

---

## ⚙️ Étape 5 : AndroidManifest.xml

```xml
<application>

    <activity android:name=".MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN"/>
            <category android:name="android.intent.category.LAUNCHER"/>
        </intent-filter>
    </activity>

    <activity android:name=".Screen2Activity"/>

</application>
```

---

## 🧪 Étape 6 : Test

1. Lancer l'application
2. Saisir les informations
3. Cliquer sur **Envoyer**
4. Vérifier l’affichage du récapitulatif
5. Cliquer sur **Retour**

---

## ⭐ Bonus (optionnel)

✔ Validation email :

```java
if (!Patterns.EMAIL_ADDRESS.matcher(sEmail).matches()) {
    Toast.makeText(this, "Email invalide", Toast.LENGTH_SHORT).show();
    return;
}
```

✔ Améliorations possibles :

* Réinitialiser les champs
* Ajouter partage (Intent ACTION_SEND)
* Ajouter Spinner pour ville

---

## 📌 Conclusion

Ce lab permet de comprendre :

* ✔ Création d’interfaces XML
* ✔ Gestion des événements (boutons)
* ✔ Passage de données avec Intent
* ✔ Navigation entre activités

---

🚀 **Lab réussi !**

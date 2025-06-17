# 📧 Sähköpostilomakkeen Asennus - Nordic Valo

## EmailJS Asennus (ILMAINEN ratkaisu)

### 1. Luo EmailJS tili
1. Mene osoitteeseen: https://www.emailjs.com/
2. Klikkaa "Sign Up" ja luo ilmainen tili
3. Vahvista sähköpostiosoitteesi

### 2. Gmail Integration (Suositeltu)
1. EmailJS dashboardissa klikkaa "Email Services"
2. Valitse "Gmail"
3. Kirjaudu Gmail-tilillesi (voit käyttää info@nordic-valo.com)
4. Anna EmailJS:lle oikeudet

### 3. Luo Email Template
1. Mene "Email Templates" -osioon
2. Klikkaa "Create New Template"
3. Käytä tätä template-sisältöä:

```
Aihe: Uusi yhteydenotto - Nordic Valo

Uusi viesti Nordic Valo -sivustolta:

Nimi: {{from_name}}
Sähköposti: {{from_email}}
Yritys: {{company}}
Puhelin: {{phone}}

Viesti:
{{message}}

---
Tämä viesti on lähetetty automaattisesti Nordic Valo -sivustolta.
```

4. Tallenna template ja kopioi Template ID

### 4. Hanki tarvittavat ID:t
- **Public Key**: Account → API Keys → Public Key
- **Service ID**: Email Services → valitse palvelusi → Service ID
- **Template ID**: Email Templates → valitse templatesi → Template ID

### 5. Päivitä sivustosi koodit

#### Nordic Valo Landing Page:
Korvaa tiedostossa `nordic-valo-strategy-landing.html`:

```javascript
// Rivillä ~5766
emailjs.init("YOUR_PUBLIC_KEY"); // ← Korvaa omalla Public Key:llä

// Rivillä ~5636
emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', templateParams)
//           ↑                   ↑
//    Service ID         Template ID
```

#### Tietosuojaseloste:
Korvaa tiedostossa `tietosuojaseloste.html`:
```javascript
emailjs.init("YOUR_PUBLIC_KEY"); // ← Sama Public Key
```

### 6. Testaa lomake
1. Tallenna muutokset
2. Lataa sivu uudelleen
3. Täytä yhteyslomake
4. Klikkaa "Lähetä Viesti"
5. Tarkista Gmail-tilisi

---

## 🔒 SSL-Sertifikaatti Ongelma

### Miksi "Ei suojattu" näkyy?

1. **Mixed Content**: Sivusto lataa HTTP-sisältöä HTTPS-sivulla
2. **Sertifikaatti ei ole oikein asennettu**
3. **DNS-asetukset eivät ole vielä päivittyneet**

### Ratkaisu 1: Tarkista hosting-asetukset

**Jos käytät cPanel/DirectAdmin:**
1. Kirjaudu hosting-paneeliin
2. Etsi "SSL/TLS" tai "Let's Encrypt"
3. Aktivoi SSL sertifikaatti domainille
4. Pakota HTTPS-ohjaus

**Jos käytät Cloudflare:**
1. Kirjaudu Cloudflare dashboardiin
2. SSL/TLS → Overview
3. Valitse "Full (strict)"
4. Edge Certificates → Always Use HTTPS = ON

### Ratkaisu 2: Force HTTPS
Lisää .htaccess-tiedostoon (juurikansioon):

```apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

### Ratkaisu 3: Tarkista koodit
Varmista, että kaikki sisältö ladataan HTTPS:llä:

```html
<!-- ✅ Oikein -->
<script src="https://fonts.googleapis.com/..."></script>

<!-- ❌ Väärin -->
<script src="http://fonts.googleapis.com/..."></script>
```

### Testaus
1. Siirry osoitteeseen: https://www.ssllabs.com/ssltest/
2. Syötä: nordicvalo.com
3. Katso raportti ja korjaa ongelmat

---

## 🚀 Vaihtoehtoinen ratkaisu: Formspree

Jos EmailJS ei toimi, voit käyttää Formspree:ä:

1. Mene: https://formspree.io/
2. Luo ilmainen tili
3. Luo uusi form
4. Korvaa lomakkeen action:

```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
```

---

## 📞 Seuraavat askeleet

1. **Asenna EmailJS** (15-30 min)
2. **Korjaa SSL-sertifikaatti** (5-60 min hosting-palvelusta riippuen)
3. **Testaa kaikki toiminnot**
4. **Lisää Google Analytics** (valinnainen)

### Tarvitsetko apua?
- EmailJS dokumentaatio: https://www.emailjs.com/docs/
- SSL testaus: https://www.ssllabs.com/ssltest/
- cPanel SSL ohje: ota yhteyttä hosting-palveluntarjoajaasi

Onnea projektin kanssa! 🎉 
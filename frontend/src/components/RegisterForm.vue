<template>
    <div class="form-container">
        <form @submit.prevent="handleRegister">
            <input type="text" v-model="username" placeholder="Nom d'utilisateur" required />
            <input type="email" v-model="email" placeholder="Email" required />
            <input type="password" v-model="password" placeholder="Mot de passe" required />
            <input type="password" v-model="confirmPassword" placeholder="Confirmer le mot de passe" required />
            <button type="submit">Créer un compte</button>
            <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
        </form>
    </div>
</template>
  

<script>
    export default {
        data() {
            return {
                username: '',
                email: '',
                password: '',
                confirmPassword: '',
                errorMessage: ''
            }
        },

        methods: {
            async handleRegister() {
                this.errorMessage = '';

                if (this.password !== this.confirmPassword) {
                    this.errorMessage = 'Les mots de passe ne correspondent pas.';
                    return;
                }

                try {
                    const response = await fetch('https://green-it-production.up.railway.app/api/register', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            username: this.username,
                            email: this.email,
                            password: this.password
                        })
                    });

                    const data = await response.json();

                    if (data.success) {
                        console.log('✅ Inscription réussie', data);
                        alert('✅ Compte créé avec succès ! Vous pouvez maintenant vous connecter.');
                        this.$emit('close');
                    } else {
                        this.errorMessage = data.message || 'Erreur lors de l\'inscription.';
                    }
                } catch (error) {
                    console.error('❌ Erreur lors de l\'inscription :', error);
                    this.errorMessage = 'Erreur de connexion au serveur.';
                }
            }
        }
    }
</script>

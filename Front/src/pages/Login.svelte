<link rel="stylesheet" type="text/css" href="/stylesheets/login.css"/>

<script>
    import { page, logado, changePage, usuario } from '../assets/js/stores';
    import { ENDPOINT_LOGIN, ENDPOINT_RECUPERAR_SENHA } from '../assets/js/endpoints';

    let email, password;

    async function logar() {
        const data = new FormData();
        data.append("email", email);
        data.append("password", password);

        const response = await fetch(ENDPOINT_LOGIN, {
            method: "POST",
            body: data,
            credentials: "include",
        });
        if (!response.ok) {
            alert("Usuário ou senha incorreto");
            return;
        }
        alert("Seja Bem-vindo(a) ao MedPlants!");
        $logado = true;
        $page = "ervas";
        $usuario = await response.json();
    }

    
	const mostrarSenha = () => {
        const senha = document.querySelector('#inputPassword');
        
		if (senha.type === 'password') {
			senha.type = 'text'

        } else {
			senha.type = 'password'
		}
	}
</script>

<div class="container">
    <div class="alert alert-success ">
        <h2 class="text-center"> Seu site para busca de ervas e plantas medicinais</h2>
    </div>
    <div class="card">
        <img id="profile-img" class="profile-img-card my-5" alt="logo" src="/images/logo.png" />

        <form class="form-signin" on:submit|preventDefault={logar}>
            <span id="reauth-email" class="reauth-email"></span>
            <div class="principal">
                Email:
                <input type="text" id="inputEmail" class="form-control" bind:value={email} placeholder="Email" required>
            </div>
            <div class="principal">
                Senha:
                <input type="password" id="inputPassword" class="form-control" bind:value={password} placeholder="Senha" required>
            </div>
            <div class="d-flex align-items-center mb-3">
                <input class="boxpasswordhide" type="checkbox" style="width: 15px" on:click={mostrarSenha}> 
                <span class="btn">Mostrar senha</span>
            </div>
            
            <div class="d-flex align-items-center justify-content-between">

                <button class="btn btn-success btn-block mr-2" type="submit">Entrar</button>
                <button class="btn btn-info" on:click={() => changePage('register')}>Cadastrar</button>
           
            </div>

        </form><!-- /form -->

        <div>
            <button class="btn btn-link" on:click={() => changePage('recuperar-senha')}>Esqueceu a senha?</button>
        </div>

    </div><!-- /card-container -->
</div>


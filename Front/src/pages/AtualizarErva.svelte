<link rel="stylesheet" type="text/css" href="/stylesheets/cadastrar-erva.css">

<script>
    import { page, changePage, ervaAtual } from "../assets/js/stores";
    import { ENDPOINT_ATUALIZAR_ERVA } from "../assets/js/endpoints";

    let nomePopular = $ervaAtual.NOME_POPULAR_ERV;
    let nomeCientifico = $ervaAtual.NOME_CIENTIFICO;
    let indicacaoUso = $ervaAtual.INDICACAO_USO_ERV;
    let contraIndicacao = $ervaAtual.CONTRA_INDICACAO_ERV;
    let propriedades = $ervaAtual.PROPRIEDADES_ERV;
    
    async function atualizar() {
        const data = new FormData();
        data.append("id_erv", $ervaAtual.ID_ERV);
        data.append("nome_popular", nomePopular);
        data.append("nome_cientifico", nomeCientifico);
        data.append("indicacao_de_uso", indicacaoUso);
        data.append("contra_indicacao_erv", contraIndicacao);
        data.append("propriedades_erv", propriedades);
        
        await fetch(ENDPOINT_ATUALIZAR_ERVA, {
            method: "POST",
            body: data,
            credentials: "include",
        });
        alert("Erva Atualizada com Sucesso!")
        $page = "ervas";
    }
</script>

<div class="container">
    <div class="card">
        <div class="mb-5 text-center">
            <h2>Atualizar Erva</h2>
        </div>
        
        <form class="form-signin" on:submit|preventDefault={atualizar}>
            <div class="row mb-2">
                <div class="col-sm-12">
                    <div class="principal">
                      Nome Popular:
                     <input type="text" class="form-control" bind:value={nomePopular} placeholder="Nome popular" required pattern="([a-zA-ZÀ-ú \-]+)" > 
                    </div>
                </div>
            </div>

            <div class="row mb-2">
              <div class="col-sm-12">
                <div class="principal">
                  Nome Científico:
                  <input type="text" class="form-control" bind:value={nomeCientifico} placeholder="Nome científico" required>
                </div>
              </div>
            </div>

            <div class="row mb-2">
              <div class="col-sm-12">
                <div class="principal">
                  Indicação de Uso:
                  <textarea class="form-control" bind:value={indicacaoUso} placeholder="Indicação de uso" required></textarea>
                </div>
              </div>
            </div>

            <div class="row mb-2">
                <div class="col-sm-12">
                  <div class="principal">
                    Contra Indicação:
                    <textarea class="form-control" bind:value={contraIndicacao} placeholder="Contra indicação" required></textarea>
                  </div>
                </div>
              </div>
              
              <div class="row mb-2">
                <div class="col-sm-12">
                  <div class="principal">
                    Propriedades:
                    <textarea class="form-control" bind:value={propriedades} placeholder="Propriedades" required></textarea>
                  </div>
                </div>
              </div>  

            <div class="row">
                <div class="col-md-10">
                    <button class="btn btn-success btn-block ml-5 mt-3" type="submit">Atualizar</button>
                </div>
                <div class="col-md-10">
                    <button class="btn btn-secondary btn-block ml-5 mt-3" on:click={() => changePage('ervas')}>Voltar</button>
                </div>
            </div>

        </form>
    </div>
</div>
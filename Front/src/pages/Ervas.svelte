<link rel="stylesheet" type="text/css" href="/stylesheets/ervas.css">

<script>
  import { onMount } from "svelte";
  import { ervas, changePage, ervaAtual } from "../assets/js/stores";
  import { ENDPOINT_LISTAR_ERVAS, ENDPOINT_DELETE_ERVA} from "../assets/js/endpoints";

  let busca;
  let ervasFiltradas = [];

  onMount(async () => {
    carregarErvas()
  });

  async function carregarErvas() {
    const response = await fetch( ENDPOINT_LISTAR_ERVAS , {
      credentials: "include",
    });
    if (response.ok) {
      $ervas = await response.json();
      ervasFiltradas = $ervas
    }
  }
  function mudarPagina (erva) {
    $ervaAtual = erva; 
    changePage("atualizar-erva");
  }
  
  function filtrar() {
    if (!busca) return null;
    
    ervasFiltradas = $ervas.filter((erva) => {
      const nome = erva.NOME_POPULAR_ERV.toLowerCase();
      const nomeCientifico = erva.NOME_CIENTIFICO.toLowerCase();
      const indicacao = erva.INDICACAO_USO_ERV.toLowerCase();
      const propriedades = erva.PROPRIEDADES_ERV.toLowerCase();  
      
      
      return nome.includes(busca.toLowerCase()) 
        || nomeCientifico.includes(busca.toLowerCase()) 
        || indicacao.includes(busca.toLowerCase())
        || propriedades.includes(busca.toLowerCase());
    });
  }
  async function deleteErva(id) {
    const form = new FormData();
    form.append("id_erv", id);
    const response = await fetch(ENDPOINT_DELETE_ERVA, {
      method: "POST",
      body: form,
      credentials: "include",
    });
    if (!response.ok) {
      alert("xii, algo deu errado!");
      return;
    }
    carregarErvas()
    alert("Erva deletada com Sucesso!")
  } 
 
</script>

<div class="container mt-2">
  <div class="card mb-2">
    <form class="row g-2 form-filter">
      <div class="col-auto">
        <label for="busca" id="busca" class="visually-hidden">Digite a erva/planta medicinal que você está buscando:</label>
        <input type="text" class="form-control" placeholder="Digite aqui" on:input={filtrar} bind:value={busca}>
    </form>
    <div class="row">
      <div class="col-sm-12">
        <button class="btn btn-success width mt-5" on:click={() => changePage('cadastrar-erva')}>
          Cadastrar nova erva
        </button>
      </div>
    </div>
    
    <div class="row text-center mt-4">
      <div class="col-sm-12">
        <table class="table table-hover table-striped table-light">
          <thead>
            <tr>
              <th>#</th>
              <th>Nome popular</th>
              <th>Nome científico</th>
              <th>Indicação de uso</th>
              <th>Contra indicação</th>
              <th>Propriedades</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody>
            {#each ervasFiltradas as erva, i }
              <tr>
                <td>{i+1}</td>
                <td>{erva.NOME_POPULAR_ERV}</td>
                <td>{erva.NOME_CIENTIFICO}</td>
                <td>{erva.INDICACAO_USO_ERV}</td>
                <td>{erva.CONTRA_INDICACAO_ERV}</td>
                <td>{erva.PROPRIEDADES_ERV}</td>
                <td>
                  <button class="btn btn-small btn-sucess" on:click={() => mudarPagina(erva)}> &#9998;</button>
                  <button class="btn btn-small btn-danger" on:click={() => deleteErva(erva.ID_ERV)}>&times;</button>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
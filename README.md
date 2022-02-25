nome: GitHub CI

em:
  pull_request:
  empurrar:
  horário:
    - cron: 0 0 * * 0

padrões:
  executar:
    shell: 'bash -Eeuo pipefail -x {0}'

empregos:

  gerar empregos:
    nome: Gerar Empregos
    em funcionamento: ubuntu-mais recente
    saídas:
      estratégia: ${{{passos.gerar-jobs.outputs.strategy }}
    passos:
      - usos: ações/checkout@v1
      - id: gerar empregos
        nome: Gerar Empregos
        corrida: |
 git clone --profundidade 1 https://github.com/docker-library/bashbrew.git -b mestre ~/bashbrew
 estratégia="$(~/bashbrew/scripts/github-actions/gerar.sh)"
 jq . <<< "$strategy" # ajuda de sanidade / depuração
 eco "::set-output name=strategy::$strategy"
  teste:
    necessidades: gerar empregos
    estratégia: ${{ fromJson(needs.generate-jobs.outputs.strategy) }}
    nome: ${{ matrix.name }}
    em funcionamento: ${{{matrix.os }}
    passos:
      - usos: ações/checkout@v1
      - nome: Preparar ambiente
        executar: ${{{matrix.runs.prepare }}
      - nome: Puxar dependências
        executar: ${{{matrix.runs.pull }}
      - nome: Build ${{ matrix.name }}
        executar: ${{{matrix.runs.build }}
      - nome: History ${{ matrix.name }}
        executar: ${{{matrix.runs.history }}
      - nome: Teste ${{ matrix.name }}
        executar: ${{{matrix.runs.test }}
      - nome: '"imagens docker"'
        executar: ${{{matrix.runs.images }}

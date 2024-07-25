# ferias
// feriasModule.js
let ferias = {};

const adicionarAtividade = (data, atividade) => {
    if (!ferias[data]) {
        ferias[data] = [];
    }
    ferias[data].push(atividade);
};

const lerAtividades = (data) => {
    return ferias[data] || [];
};

const atualizarAtividade = (data, index, novaAtividade) => {
    if (ferias[data] && ferias[data][index]) {
        ferias[data][index] = novaAtividade;
        return true;
    }
    return false;
};

const deletarAtividade = (data, index) => {
    if (ferias[data] && ferias[data][index]) {
        ferias[data].splice(index, 1);
        if (ferias[data].length === 0) {
            delete ferias[data];
        }
        return true;
    }
    return false;
};

module.exports = {
    adicionarAtividade,
    lerAtividades,
    atualizarAtividade,
    deletarAtividade,
};


// main.js
const prompt = require('prompt-sync')();
const feriasModule = require('./feriasModule');

const menu = () => {
    console.log("\n".repeat(2));
    console.log(" ".repeat(20));
    console.log("Gerenciador de Atividades nas Férias");
    console.log("1. Adicionar Atividade");
    console.log("2. Ler Atividades");
    console.log("3. Atualizar Atividade");
    console.log("4. Deletar Atividade");
    console.log("5. Sair");
    console.log(" ".repeat(20));
    return prompt("Escolha uma opção: ");
};

const adicionarAtividade = () => {
    const data = prompt("Digite a data (dd/mm): ");
    const atividade = prompt("Digite a atividade: ");
    feriasModule.adicionarAtividade(data, atividade);
    console.log("Atividade adicionada!");
};

const lerAtividades = () => {
    const data = prompt("Digite a data (dd/mm): ");
    const atividades = feriasModule.lerAtividades(data);
    if (atividades.length > 0) {
        console.log(`Atividades do dia ${data}:`);
        atividades.forEach((atividade, index) => {
            console.log(`${index + 1}. ${atividade}`);
        });
    } else {
        console.log("Nenhuma atividade encontrada para esta data.");
    }
};

const atualizarAtividade = () => {
    const data = prompt("Digite a data (dd/mm): ");
    const atividades = feriasModule.lerAtividades(data);
    if (atividades.length > 0) {
        atividades.forEach((atividade, index) => {
            console.log(`${index + 1}. ${atividade}`);
        });
        const index = parseInt(prompt("Digite o número da atividade que deseja atualizar: ")) - 1;
        const novaAtividade = prompt("Digite a nova atividade: ");
        if (feriasModule.atualizarAtividade(data, index, novaAtividade)) {
            console.log("Atividade atualizada!");
        } else {
            console.log("Erro ao atualizar atividade.");
        }
    } else {
        console.log("Nenhuma atividade encontrada para esta data.");
    }
};

const deletarAtividade = () => {
    const data = prompt("Digite a data (dd/mm): ");
    const atividades = feriasModule.lerAtividades(data);
    if (atividades.length > 0) {
        atividades.forEach((atividade, index) => {
            console.log(`${index + 1}. ${atividade}`);
        });
        const index = parseInt(prompt("Digite o número da atividade que deseja deletar: ")) - 1;
        if (feriasModule.deletarAtividade(data, index)) {
            console.log("Atividade deletada!");
        } else {
            console.log("Erro ao deletar atividade.");
        }
    } else {
        console.log("Nenhuma atividade encontrada para esta data.");
    }
};

let opcao;
do {
    opcao = menu();
    switch (opcao) {
        case '1':
            adicionarAtividade();
            break;
        case '2':
            lerAtividades();
            break;
        case '3':
            atualizarAtividade();
            break;
        case '4':
            deletarAtividade();
            break;
        case '5':
            console.log("Saindo...");
            break;
        default:
            console.log("Opção inválida!");
    }
} while (opcao !== '5');





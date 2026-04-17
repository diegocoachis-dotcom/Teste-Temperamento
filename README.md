# Teste-Temperamento
Teste de temperamento gratuito.
// temperament.js
/**
 * Função principal que calcula o temperamento baseado nas respostas.
 * @param {string[]} answers - Array de 12 respostas: 'A', 'B', 'C' ou 'D'.
 * @returns {object} - Objeto com temperamento, características e resumo de áreas.
 */
function scoreTemperament(answers) {
  if (!Array.isArray(answers) || answers.length !== 12) {
    throw new Error('Array de respostas deve ter exatamente 12 itens válidos.');
  }

  // Contagem de pontuações: A=Colérico, B=Sanguíneo, C=Fleumático, D=Melancólico
  const scores = { colerico: 0, sanguineo: 0, fleumatico: 0, melancolico: 0 };
  answers.forEach(answer => {
    if (answer === 'A') scores.colerico++;
    else if (answer === 'B') scores.sanguineo++;
    else if (answer === 'C') scores.fleumatico++;
    else if (answer === 'D') scores.melancolico++;
    else throw new Error('Resposta inválida: deve ser A, B, C ou D.');
  });

  // Determinação do temperamento dominante (argmax das pontuações)
  // Em caso de empate, retorna o primeiro na ordem: Colérico, Sanguíneo, Fleumático, Melancólico
  const temperamentMap = {
    colerico: 'Colérico',
    sanguineo: 'Sanguíneo',
    fleumatico: 'Fleumático',
    melancolico: 'Melancólico'
  };
  const order = ['colerico', 'sanguineo', 'fleumatico', 'melancolico'];
  let maxScore = 0;
  let dominant = 'colerico';
  order.forEach(key => {
    if (scores[key] > maxScore) {
      maxScore = scores[key];
      dominant = key;
    }
  });

  // Dados hardcoded por temperamento
  const data = {
    colerico: {
      caracteristicas: ['Líder nato', 'Energético', 'Decisivo', 'Competitivo', 'Impaciente'],
      resumoAreas: 'No amor, os coléricos são apaixonados e dominantes, buscando controle e intensidade nas relações. Nas finanças, são empreendedores arriscados, investindo em oportunidades de alto retorno. No trabalho, atuam como líderes naturais, motivando equipes com energia e determinação. Em amizades, preferem grupos dinâmicos e competitivos. Na saúde, precisam controlar o estresse para evitar explosões de raiva. São éticos ao defender justiça e lealdade, mas podem ser impacientes com processos lentos.'
    },
    sanguineo: {
      caracteristicas: ['Extrovertido', 'Alegre', 'Sociável', 'Criativo', 'Impulsivo'],
      resumoAreas: 'No amor, os sanguíneos são românticos e divertidos, valorizando aventuras e risadas. Nas finanças, gastam impulsivamente, priorizando prazer imediato. No trabalho, brilham em vendas e marketing, inspirando entusiasmo. Em amizades, são o centro das atenções, conectando pessoas facilmente. Na saúde, cuidam da energia social, evitando isolamento. São éticos ao promover harmonia e positividade, mas precisam equilibrar impulsos para evitar arrependimentos.'
    },
    fleumatico: {
      caracteristicas: ['Calmo', 'Paciente', 'Leal', 'Diplomático', 'Preguiçoso'],
      resumoAreas: 'No amor, os fleumáticos são estáveis e leais, preferindo relações calmas e duradouras. Nas finanças, são conservadores, evitando riscos desnecessários. No trabalho, excelam em papéis de suporte, resolvendo conflitos com diplomacia. Em amizades, são ouvintes confiáveis, mantendo laços profundos. Na saúde, priorizam equilíbrio, mas podem procrastinar cuidados. São éticos ao valorizar paz e justiça, contribuindo para ambientes harmoniosos sem conflitos.'
    },
    melancolico: {
      caracteristicas: ['Pensativo', 'Perfeccionista', 'Sensível', 'Organizado', 'Triste'],
      resumoAreas: 'No amor, os melancólicos são profundos e leais, buscando conexões emocionais intensas. Nas finanças, são cuidadosos e planejadores, evitando dívidas. No trabalho, destacam-se em detalhes e análise, garantindo qualidade. Em amizades, são seletivos, valorizando amizades verdadeiras. Na saúde, lidam com sensibilidade emocional, precisando de apoio. São éticos ao defender princípios e empatia, contribuindo para causas sociais com dedicação.'
    }
  };

  return {
    temperamento: temperamentMap[dominant],
    caracteristicas: data[dominant].caracteristicas,
    resumoAreas: data[dominant].resumoAreas
  };
}

module.exports = { scoreTemperament };

// temperament.test.js
const { scoreTemperament } = require('./temperament');

describe('scoreTemperament', () => {
  let originalConsoleError;

  beforeEach(() => {
    // Limpa mocks se necessário (não aplicável aqui)
  });

  afterAll(() => {
    // Limpa após todos os testes (não aplicável aqui)
  });

  it('calcula pontuação Colérico dominante', () => {
    const answers = Array(12).fill('A');
    const result = scoreTemperament(answers);
    expect(result.temperamento).toBe('Colérico');
    expect(result.caracteristicas).toEqual(['Líder nato', 'Energético', 'Decisivo', 'Competitivo', 'Impaciente']);
    expect(result.resumoAreas).toContain('No amor, os coléricos');
  });

  it('calcula pontuação Sanguíneo dominante', () => {
    const answers = Array(12).fill('B');
    const result = scoreTemperament(answers);
    expect(result.temperamento).toBe('Sanguíneo');
    expect(result.caracteristicas).toEqual(['Extrovertido', 'Alegre', 'Sociável', 'Criativo', 'Impulsivo']);
    expect(result.resumoAreas).toContain('No amor, os sanguíneos');
  });

  it('calcula pontuação Fleumático dominante', () => {
    const answers = Array(12).fill('C');
    const result = scoreTemperament(answers);
    expect(result.temperamento).toBe('Fleumático');
    expect(result.caracteristicas).toEqual(['Calmo', 'Paciente', 'Leal', 'Diplomático', 'Preguiçoso']);
    expect(result.resumoAreas).toContain('No amor, os fleumáticos');
  });

  it('calcula pontuação Melancólico dominante', () => {
    const answers = Array(12).fill('D');
    const result = scoreTemperament(answers);
    expect(result.temperamento).toBe('Melancólico');
    expect(result.caracteristicas).toEqual(['Pensativo', 'Perfeccionista', 'Sensível', 'Organizado', 'Triste']);
    expect(result.resumoAreas).toContain('No amor, os melancólicos');
  });

  it('retorna primeiro max em caso de empate', () => {
    const answers = Array(6).fill('A').concat(Array(6).fill('B')); // 6A, 6B
    const result = scoreTemperament(answers);
    expect(result.temperamento).toBe('Colérico'); // Primeiro na ordem
  });

  it('lança erro para array vazio', () => {
    expect(() => scoreTemperament([])).toThrow('Array de respostas deve ter exatamente 12 itens válidos.');
  });

  it('lança erro para array com itens inválidos', () => {
    const answers = Array(11).fill('A').concat(['X']);
    expect(() => scoreTemperament(answers)).toThrow('Resposta inválida: deve ser A, B, C ou D.');
  });
});
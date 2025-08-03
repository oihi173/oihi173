import React, { useState } from 'react';

type Pessoa = {
  id: number;
  nome: string;
  distancia: number; // Em metros
};

type Props = {
  personagem: { nome: string };
  pessoasProximas: Pessoa[];
  onAbracar: (pessoa: Pessoa) => void;
};

const PainelAbraco: React.FC<Props> = ({ personagem, pessoasProximas, onAbracar }) => {
  const [aberto, setAberto] = useState(false);

  // Ordena as pessoas mais próximas
  const pessoasOrdenadas = [...pessoasProximas].sort((a, b) => a.distancia - b.distancia);

  return (
    <div>
      <button
        style={{
          background: 'gold',
          color: 'black',
          border: 'none',
          padding: '10px 20px',
          borderRadius: 5,
          fontWeight: 'bold',
          cursor: 'pointer',
        }}
        onClick={() => setAberto(!aberto)}
      >
        {aberto ? "Fechar Painel" : "Abrir Painel Abraço"}
      </button>

      {aberto && (
        <div
          style={{
            background: 'yellow',
            border: '2px solid #bbb700',
            marginTop: 20,
            padding: 20,
            borderRadius: 10,
            width: 300,
            boxShadow: '0 2px 10px #bbb70055'
          }}
        >
          <h3>Painel do Abraço 🤗</h3>
          <p>{personagem.nome}, escolha alguém para abraçar:</p>
          {pessoasOrdenadas.length === 0 ? (
            <div>Ninguém por perto!</div>
          ) : (
            <ul style={{ padding: 0 }}>
              {pessoasOrdenadas.map((pessoa) => (
                <li key={pessoa.id} style={{ listStyle: 'none', marginBottom: 8 }}>
                  <button
                    style={{
                      background: '#fffbe6',
                      border: '1px solid #ffc300',
                      borderRadius: 5,
                      padding: '5px 15px',
                      cursor: 'pointer',
                      marginRight: 8
                    }}
                    onClick={() => onAbracar(pessoa)}
                  >
                    Abraçar {pessoa.nome} ({pessoa.distancia}m)
                  </button>
                </li>
              ))}
            </ul>
          )}
        </div>
      )}
    </div>
  );
};

export default PainelAbraco;

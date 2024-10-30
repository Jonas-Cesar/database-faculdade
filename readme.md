# Database Faculdade
Esse repositorio tem como objetivo apresentar minha atividade da faculdade que consiste na modelagem de um banco da dados de uma faculdade a partir do seguinte estudo de caso:

"Os proprietários de uma faculdade precisam de um sistema que viabilize o armazenamento de informações sobre seus alunos, cursos,matérias professores para que seja possível realizar controles básicos como montar turmas e realizar o armazenamento dos alunos."

## O que você vai encontrar

* Levantamento de Requisitos
* Modelo Conceitual
* Modelo Lógico
* Modelo Físico

# Levantamento de requisitos
1. Quais são as principais necessidades dos clientes?

    * Resposta: Os proprietários de uma faculdade precisam de um sistema para armazenar informações sobre alunos, cursos, matérias e professores, permitindo controlar turmas e notas dos alunos.

2. Quais informações precisam ser armazenadas?
    * Resposta: Informações que precisam ser armazenadas são dos alunos, cursos, matérias, professores, turmas e notas dos alunos.

3. Dados que precisam ser guardados?
    * Resposta: Alunos: Nome, CPF, data de nascimento, endereço, telefone, e-mail e curso. Cursos: nome do curso, carga horária e  coordenador. Matérias: nome da matéria, carga horária e professor responsável. Professores: nome, CPF, data de nascimento, área de atuação, endereço, telefone e e-mail. Turmas: curso, matéria, professor, horário e sala. Notas: aluno, matéria, nota, data da avaliação.


4. O que será feito com os dados posteriormente?
    * Resposta: Controle de turmas, armazenamento de notas, consulta de informações de alunos, cursos, matérias e professores. Relatórios e análises acadêmicas.

5. Quais tabelas precisam ser criadas para que todas as informações sejam armazenadas?
    * Resposta: tabela de alunos, tabela de cursos, tabela das matérias, tabela dos professores, tabela das turno e tabela das notas.

6.  Quais atributos cada tabela deve ter?
    * Resposta: Tabela de alunos: id, nome, cpf, data_nasc, endereco, telefone, email, curso_id. Tabela de cursos: id, nome, carga_horaria, coordenador. Tabela de matérias: id, nome, carga_horaria, professor_id. Tabela de professores: id,  nome, cpf , data_nasc, area_atuacao, endereco, telefone, email. Tabela de turmas: id, curso_id, materia_id, professor_id, horario, sala. Tabela de notas: id, aluno_id, materia_id, nota, data_avaliacao.

7. Qual tipo de dados de cada atributo definido?
    * Resposta: id (INT, PRIMARY KEY, AUTO_INCREMENT), nome (TEXT), CPF (VARCHAR(20), UNIQUE), data_nasc (DATE), endereco (NOVA TABELA), telefone (NOVA TABELA), email (TEXT), curso_id (INT, FOREIGN KEY), carga_horaria (TIME), coordenador (TEXT), professor_id (INT, FOREIGN KEY), area_atuacao (VARCHAR(45)), curso_id (INT, FOREIGN KEY), materia_id (INT, FOREIGN KEY), horario (TIME), sala (VARCHAR(20)), aluno_id (INT, FOREIGN KEY), nota (FLOAT)), data_avaliacao (DATE), 


8. Quais são os relacionamentos a serem criados entre as tabelas?
    * Resposta: A Tabela de alunos vai ter uma relação de muitos para um com tabela de cursos. A tabela de matérias vai ter uma relação de muitos para um com a tabela de professores. A tabela de turmas vai ter relações de um para um com a tabela de cursos, um para muitos com a tabela de matérias e tabela de professores. A tabela de notas vai ter relações de um para muitos com tabela de alunos e tabela de matérias.
  
## Modelo Conceitual
<img src="modelo_conceitual_fac.png">

## Modelo Lógico
<img src="modelo_logico_fac.png">

## Modelo Físico

CREATE DATABASE facsystem <br>
USE facsystem;

CREATE TABLE tbl_cursos ( <br>
id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
nome TEXT NOT NULL, <br>
coordenador TEXT NOT NULL, <br>
carga_horaria TIME NOT NULL, <br>
PRIMARY KEY (id), <br>
UNIQUE INDEX (id) <br>
)ENGINE = InnoDB;


CREATE TABLE tbl_professores ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  nome TEXT NOT NULL, <br>
  cpf VARCHAR(20) NOT NULL, <br>
  data_nasc DATE NOT NULL, <br>
  area_atuacao VARCHAR(45) NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX (id), <br>
  UNIQUE INDEX (cpf) <br>
  )ENGINE = InnoDB;


CREATE TABLE tbl_materias ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  nome TEXT NOT NULL, <br>
  carga_horaria TIME NOT NULL, <br>
  id_professor INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX (id), <br>
  INDEX fk_tbl_materias_tbl_professores1_idx (id_professor), <br>
  CONSTRAINT fk_tbl_materias_tbl_professores1 <br>
    FOREIGN KEY (id_professor) <br>
    REFERENCES tbl_professores (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION <br>
    )ENGINE = InnoDB;

CREATE TABLE tbl_turmas ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  sala VARCHAR(30) NOT NULL, <br>
  horario TIME NOT NULL, <br>
  id_curso INT UNSIGNED NOT NULL, <br>
  id_professor INT UNSIGNED NOT NULL, <br>
  id_turma INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX (id), <br>
  INDEX fk_tbl_turmas_tbl_cursos_idx (id_curso), <br>
  INDEX fk_tbl_turmas_tbl_professores1_idx (id_professor), <br>
  INDEX fk_tbl_turmas_tbl_materias1_idx (id_turma), <br>
  CONSTRAINT fk_tbl_turmas_tbl_cursos <br>
    FOREIGN KEY (id_curso) <br>
    REFERENCES tbl_cursos (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION, <br>
  CONSTRAINT fk_tbl_turmas_tbl_professores1 <br>
    FOREIGN KEY (id_professor) <br>
    REFERENCES tbl_professores (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION, <br>
  CONSTRAINT fk_tbl_turmas_tbl_materias1 <br>
    FOREIGN KEY (id_turma) <br>
    REFERENCES tbl_materias (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION <br>
    )ENGINE = InnoDB;

CREATE TABLE tbl_alunos ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  nome TEXT NOT NULL, <br>
  cpf VARCHAR(20) NOT NULL, <br>
  data_nasc DATE NOT NULL, <br>
  id_curso INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX (id), <br>
  UNIQUE INDEX (cpf), <br>
  INDEX fk_tbl_alunos_tbl_cursos1_idx (id_curso), <br>
  CONSTRAINT fk_tbl_alunos_tbl_cursos1 <br>
    FOREIGN KEY (id_curso) <br>
    REFERENCES tbl_cursos (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION <br>
    )ENGINE = InnoDB;

CREATE TABLE tbl_endereco_alunos ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  cidade TEXT NOT NULL, <br>
  bairro TEXT NOT NULL, <br>
  rua TEXT NOT NULL, <br>
  id_aluno INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX id_UNIQUE (id), <br>
  INDEX fk_tbl_endereco_alunos_tbl_alunos1_idx (id_aluno), <br>
  CONSTRAINT fk_tbl_endereco_alunos_tbl_alunos1 <br>
    FOREIGN KEY (id_aluno) <br>
    REFERENCES tbl_alunos (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION <br>
    )ENGINE = InnoDB;

CREATE TABLE tbl_telefones_alunos ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  numero_tel VARCHAR(20) NOT NULL, <br>
  id_aluno INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX (numero_tel), <br>
  INDEX fk_tbl_telefones_alunos_tbl_alunos1_idx (id_aluno), <br>
  CONSTRAINT fk_tbl_telefones_alunos_tbl_alunos1 <br>
    FOREIGN KEY (id_aluno) <br>
    REFERENCES tbl_alunos (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION <br>
    )ENGINE = InnoDB;


CREATE TABLE tbl_emails_alunos ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  email TEXT NOT NULL, <br>
  id_aluno INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX (id), <br>
  INDEX fk_tbl_emails_alunos_tbl_alunos1_idx (id_aluno), <br>
  CONSTRAINT fk_tbl_emails_alunos_tbl_alunos1 <br>
    FOREIGN KEY (id_aluno) <br>
    REFERENCES tbl_alunos (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION) <br>
ENGINE = InnoDB;


CREATE TABLE endereco_professores ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  ruia TEXT NOT NULL, <br>
  bairro TEXT NOT NULL, <br>
  cidade TEXT NOT NULL, <br>
  id_professores INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX id_UNIQUE (id), <br>
  INDEX fk_endereco_professores_tbl_professores1_idx (id_professores), <br>
  CONSTRAINT fk_endereco_professores_tbl_professores1 <br>
    FOREIGN KEY (id_professores) <br>
    REFERENCES tbl_professores (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION) <br>
ENGINE = InnoDB;


CREATE TABLE IF NOT EXISTS tbl_telefones_professores ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  numero_tel VARCHAR(20) NOT NULL, <br>
  id_professor INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX id_UNIQUE (id), <br>
  UNIQUE INDEX numero_tel_UNIQUE (numero_tel), <br>
  INDEX fk_tbl_telefones_professores_tbl_professores1_idx (id_professor), <br>
  CONSTRAINT fk_tbl_telefones_professores_tbl_professores1 <br>
    FOREIGN KEY (id_professor) <br>
    REFERENCES tbl_professores (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION) <br>
ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS tbl_emails_professores ( <br>
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  email TEXT NOT NULL, <br>
  id_professor INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX id_UNIQUE (id), <br>
  INDEX fk_tbl_emails_professores_tbl_professores1_idx (id_professor), <br>
  CONSTRAINT fk_tbl_emails_professores_tbl_professores1 <br>
    FOREIGN KEY (id_professor) <br>
    REFERENCES tbl_professores (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION) <br>
ENGINE = InnoDB;

CREATE TABLE tbl_notas (
  id INT UNSIGNED NOT NULL AUTO_INCREMENT, <br>
  nota FLOAT UNSIGNED NOT NULL, <br>
  data_avalicao DATE NOT NULL, <br>
  id_materia INT UNSIGNED NOT NULL, <br>
  id_alunos INT UNSIGNED NOT NULL, <br>
  PRIMARY KEY (id), <br>
  UNIQUE INDEX id_UNIQUE (id), <br>
  INDEX fk_tbl_notas_tbl_materias1_idx (id_materia), <br>
  INDEX fk_tbl_notas_tbl_alunos1_idx (id_alunos), <br>
  CONSTRAINT fk_tbl_notas_tbl_materias1 <br>
    FOREIGN KEY (id_materia) <br>
    REFERENCES tbl_materias (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION, <br>
  CONSTRAINT fk_tbl_notas_tbl_alunos1 <br>
    FOREIGN KEY (id_alunos) <br>
    REFERENCES tbl_alunos (id) <br>
    ON DELETE NO ACTION <br>
    ON UPDATE NO ACTION <br>
    )ENGINE = InnoDB;

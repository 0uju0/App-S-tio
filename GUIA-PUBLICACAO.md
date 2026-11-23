# Guia de publicação: Meu Sítio

## Ordem para publicar (importante)
1. **Regras primeiro.** Firebase > Firestore > Regras > cole o arquivo `firestore.rules` > Publicar.
2. **Depois o app.** GitHub > meu-sitio > Add file > Upload files: `index.html`, `privacidade.html`, `termos.html` (mantém os demais) > Commit.
3. No celular, feche e abra o app duas vezes (para trazer a versão nova). O Perfil deve mostrar "versão 6".

## Antes de divulgar (só você pode fazer)
- [ ] Abrir `privacidade.html` e `termos.html` no GitHub (lápis) e trocar `[SEU NOME COMPLETO]` e `[SEU E-MAIL DE CONTATO]`.
- [ ] Pedir a um advogado para revisar os dois textos (são rascunhos).
- [ ] Testar as regras (abaixo) e fazer o teste com duas contas reais.
- [ ] App Check (grátis): Firebase > App Check > Apps > registrar o app web com **reCAPTCHA v3** (chave em google.com/recaptcha/admin, domínio `0uju0.github.io`). Cole a chave pública em `APPCHECK_KEY` no `index.html`. Use o app alguns dias, confira em App Check > APIs que o tráfego está "verificado" e só então clique em **Aplicar** no Firestore.
- [ ] Authentication > Settings > Authorized domains: deixar só `0uju0.github.io` e `sitio-bom-jesus.firebaseapp.com`.
- [ ] Google Cloud > APIs e serviços > Credenciais: restringir a chave de API do navegador ao referenciador `https://0uju0.github.io/*`.

## Teste das regras (Firestore > Regras > Playground)
Rodar com e sem autenticação:
- Sem login: ler `sitios/qualquer` → **negado**.
- Login como estranho (uid que não está em memberUids): ler `sitios/ID` e `sitios/ID/itens/x` → **negado**.
- Funcionário (papel `funcionario` em membrosInfo): ler `sitios/ID/lancamentos/x` → **negado**; ler `sitios/ID/itens/x` → **permitido**.
- Gestor: criar `lancamentos` com valor `-5` → **negado**; com valor `10`, tipo `gasto`, cat e desc texto, em número → **permitido**.
- Dono: excluir `sitios/ID` → **permitido**; gestor → **negado**.

## Teste com duas contas
1. Conta A (dono) convida a conta B como **funcionário**. B aceita.
2. B não vê Financeiro nem campos de valor; consegue repor paiol, registrar água e manejo.
3. A troca B para **gestor**: B passa a ver o Financeiro.
4. B recusa/sai e perde o acesso na hora.
5. Em Perfil: "Apagar minha conta e meus dados" numa conta de teste: confirme que os dados somem no console.

## O que já está no app
- Papéis: dono, gestor, funcionário (Financeiro, custos e funcionários só para dono/gestor, tanto na tela quanto nas regras).
- Regras com validação de formato, limites de tamanho e exclusão de propriedade só pelo dono.
- Privacidade e Termos (rascunhos), aceite na tela de login, links no Perfil.
- Exportar dados, apagar conta (apaga as propriedades do dono e remove você das demais), excluir propriedade, "sair e apagar dados do aparelho".
- Lembrete semanal de backup para o dono.

## Fica para depois (exige plano pago ou serviço externo)
- Backup automático diário e recuperação a qualquer momento (PITR): exige plano Blaze (pago).
- Alerta de orçamento: exige conta de faturamento (Blaze). Ative **antes** de ligar o Blaze, com limite baixo.
- Notificações no celular e Telegram, resumo enviado por e-mail, convite por e-mail: exigem Cloud Functions (Blaze).
- Integração com aeradores e sensores: exige hardware e servidor; guarde chaves fora do GitHub público.
- Publicar na Play Store (TWA): taxa única de US$ 25 do Google Play.
- Domínio próprio (opcional).
- Ativar a contagem de XP no lançamento (`XP_ATIVO=true`).

## Limites honestos
- As regras novas **não foram executadas contra o Firebase real** (o emulador não roda aqui). O teste do Playground e das duas contas é obrigatório.
- Os textos legais são rascunhos, não aconselhamento jurídico.
- Backups: enquanto não houver Blaze, o único backup é o arquivo baixado pelo app. Faça isso toda semana.

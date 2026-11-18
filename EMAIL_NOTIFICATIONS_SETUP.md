# Configurar avisos por e-mail de exclusão

A página grava o pedido em `deletion_requests`, com destinatário e conteúdo fixos e validados pelas regras do Firestore. Para entregar o e-mail, instale a extensão oficial **Trigger Email from Firestore** (`firebase/firestore-send-email`) no projeto `halogen-height-85gvv`.

## Configuração da extensão

No Console Firebase, abra **Extensions** e instale **Trigger Email from Firestore**. Use:

- **Firestore instance ID:** `ai-studio-fruitvocabmerge-3eee0cc0-a7cf-4d3f-a7be-1075597607d2`
- **Firestore instance location:** selecione a localização desse banco.
- **Email documents collection:** `deletion_requests`
- **Default FROM address:** `henriquesousab@gmail.com`
- **SMTP connection URI (Gmail):** `smtps://henriquesousab@gmail.com@smtp.gmail.com:465`
- **SMTP password:** uma senha de app do Google para essa conta. Não use nem publique sua senha normal.
- **Authentication type:** Username & Password
- **TTL:** Never (os pedidos não devem expirar automaticamente antes do processamento).

A conta Gmail usada para envio precisa ter a verificação em duas etapas ativada e uma senha de app criada. Guarde a senha somente no campo secreto solicitado pelo Firebase.

## Regras

Publique o bloco em [firestore-deletion-requests.rules.txt](firestore-deletion-requests.rules.txt) na aba **Segurança** do mesmo banco. Mantenha o default-deny e as demais regras do app. O bloco fixa o destinatário e o conteúdo para que o cliente não consiga usar a extensão para enviar mensagens arbitrárias.

## Faturamento

A extensão oficial exige o plano Blaze e uma conta de faturamento vinculada. A extensão usa Cloud Functions e um servidor SMTP. Confira os custos e configure alertas de orçamento antes de ativar o faturamento.

Depois que a extensão estiver instalada e as regras publicadas, cada nova solicitação deve criar um documento com estado de entrega na coleção `deletion_requests`. Você pode conferir `delivery.state` e `delivery.error` no documento para verificar o envio.

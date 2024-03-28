Suite à l'installation de Ping Castle et Purple Knight nous avons eu deux audits, dans ce UserGuide nous allons partager avec vous diverses problèmes que ses audits nous ont remontés les audits et les moyens de résolution.

# Ping Castle
## "The LAN Manager Authentication Level allows the use of NTLMv1 or LM"

Pour résoudre ce problème, nous allons mettre en place une GPO qui va changer les paramètres par défaut de NTLM. 

Crée une GPO avec votre bonne pratiques et appliqué les règles suivantes : 

Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options > LAN Manager Authentication > "Send NTLMv2 response only. Refuse LM & NTLM"

## "Non-admin users can add up to 10 computer(s) of the domain"

Pour résoudre se problème, nous allons mettre en place une GPO qui va changer autorisé un groupe d'utilisateur qui géreront l'intégration des ordinateurs dans l'AD
Crée une GPO avec votre bonne pratiques et appliqué les règles suivantes : 

"Computer Configuration > Windows Settings > Security Settings > Local Policies > User Rights Assignement  > Add workstations to domain > Nous renseignons le groupe DSI dans notre cas" 

## "Number of DC with a configuration issue : 2"


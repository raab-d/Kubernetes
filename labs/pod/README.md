# Lab POD

Listez les pods présents sur votre cluster
```bash
kubectl get pod
```

Regardez le code pour créer un pod 
```bash
cat pod/monolith.yaml
```

Qu'est-ce qui est décrit dans ce fichier YAML ?

Créer le pod : 
```bash
kubectl create -f  pod/monolith.yaml
```

Listez à nouveau les pods présents sur votre cluster ! 

En utilisant kubectl --help, déterminez la commande pour afficher les détails de votre pod.
 * Quelle est la commande que vous avez utilisée ?
 * Quelle est l’ip du pod ? 
 * Sur quel node le pod tourne ?
 * Quel container tourne dans le pod ?


Créer un tunnel depuis son poste avec le pod
```bash
kubectl port-forward monolith 10080:80
```

Ouvrir une autre fenêtre shell
```bash
curl http://127.0.0.1:10080
curl http://127.0.0.1:10080/secure
curl -u user http://127.0.0.1:10080/login
# Le mot de passe est : "password"
curl -H "Authorization: Bearer <token>" http://127.0.0.1:10080/secure
```

Affichier les logs du pods ? 

Ouvrir un shell dans le pod 
```bash
kubectl exec monolith --stdin --tty -c monolith /bin/sh
```

Lister les process dans le container ? 


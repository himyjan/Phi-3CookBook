<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "e46691923dca7cb2f11d32b1d9d558e0",
  "translation_date": "2025-07-16T20:50:39+00:00",
  "source_file": "md/01.Introduction/03/Kaito_Inference.md",
  "language_code": "el"
}
-->
## Εκτέλεση inference με το Kaito

Το [Kaito](https://github.com/Azure/kaito) είναι ένας operator που αυτοματοποιεί την ανάπτυξη μοντέλων AI/ML για inference σε ένα Kubernetes cluster.

Το Kaito έχει τα εξής βασικά πλεονεκτήματα σε σχέση με τις περισσότερες από τις συνηθισμένες μεθόδους ανάπτυξης μοντέλων που βασίζονται σε υποδομές εικονικών μηχανών:

- Διαχείριση αρχείων μοντέλων μέσω container images. Παρέχεται ένας http server για την εκτέλεση κλήσεων inference χρησιμοποιώντας τη βιβλιοθήκη μοντέλων.
- Αποφυγή ρύθμισης παραμέτρων ανάπτυξης για να ταιριάζουν στο υλικό GPU, μέσω προκαθορισμένων ρυθμίσεων.
- Αυτόματη παροχή κόμβων GPU βάσει των απαιτήσεων του μοντέλου.
- Φιλοξενία μεγάλων εικόνων μοντέλων στο δημόσιο Microsoft Container Registry (MCR), εφόσον το επιτρέπει η άδεια χρήσης.

Με το Kaito, η διαδικασία ενσωμάτωσης μεγάλων μοντέλων AI για inference σε Kubernetes απλοποιείται σημαντικά.

## Αρχιτεκτονική

Το Kaito ακολουθεί το κλασικό πρότυπο σχεδίασης Kubernetes Custom Resource Definition (CRD)/controller. Ο χρήστης διαχειρίζεται έναν custom resource `workspace` που περιγράφει τις απαιτήσεις GPU και τις προδιαγραφές του inference. Οι controllers του Kaito αυτοματοποιούν την ανάπτυξη, συγχρονίζοντας τον custom resource `workspace`.
<div align="left">
  <img src="https://github.com/kaito-project/kaito/blob/main/docs/img/arch.png" width=80% title="Kaito architecture" alt="Kaito architecture">
</div>

Η παραπάνω εικόνα παρουσιάζει μια επισκόπηση της αρχιτεκτονικής του Kaito. Τα βασικά της στοιχεία είναι:

- **Workspace controller**: Συγχρονίζει τον custom resource `workspace`, δημιουργεί custom resources `machine` (περιγράφονται παρακάτω) για να ενεργοποιήσει την αυτόματη παροχή κόμβων, και δημιουργεί το workload inference (`deployment` ή `statefulset`) βάσει των προκαθορισμένων ρυθμίσεων του μοντέλου.
- **Node provisioner controller**: Ο controller ονομάζεται *gpu-provisioner* στο [gpu-provisioner helm chart](https://github.com/Azure/gpu-provisioner/tree/main/charts/gpu-provisioner). Χρησιμοποιεί το CRD `machine` που προέρχεται από το [Karpenter](https://sigs.k8s.io/karpenter) για να αλληλεπιδράσει με τον workspace controller. Ενσωματώνεται με τα APIs του Azure Kubernetes Service (AKS) για να προσθέτει νέους κόμβους GPU στο AKS cluster.  
> Σημείωση: Το [*gpu-provisioner*](https://github.com/Azure/gpu-provisioner) είναι ένα ανοιχτού κώδικα συστατικό. Μπορεί να αντικατασταθεί από άλλους controllers εφόσον υποστηρίζουν τα APIs του [Karpenter-core](https://sigs.k8s.io/karpenter).

## Εγκατάσταση

Παρακαλώ δείτε τις οδηγίες εγκατάστασης [εδώ](https://github.com/Azure/kaito/blob/main/docs/installation.md).

## Γρήγορη εκκίνηση Inference Phi-3
[Παράδειγμα κώδικα Inference Phi-3](https://github.com/Azure/kaito/tree/main/examples/inference)

```
apiVersion: kaito.sh/v1alpha1
kind: Workspace
metadata:
  name: workspace-phi-3-mini
resource:
  instanceType: "Standard_NC6s_v3"
  labelSelector:
    matchLabels:
      apps: phi-3
inference:
  preset:
    name: phi-3-mini-4k-instruct
    # Note: This configuration also works with the phi-3-mini-128k-instruct preset
```

```sh
$ cat examples/inference/kaito_workspace_phi_3.yaml

apiVersion: kaito.sh/v1alpha1
kind: Workspace
metadata:
  name: workspace-phi-3-mini
resource:
  instanceType: "Standard_NC6s_v3"
  labelSelector:
    matchLabels:
      app: phi-3-adapter
tuning:
  preset:
    name: phi-3-mini-4k-instruct
  method: qlora
  input:
    urls:
      - "https://huggingface.co/datasets/philschmid/dolly-15k-oai-style/resolve/main/data/train-00000-of-00001-54e3756291ca09c6.parquet?download=true"
  output:
    image: "ACR_REPO_HERE.azurecr.io/IMAGE_NAME_HERE:0.0.1" # Tuning Output ACR Path
    imagePushSecret: ACR_REGISTRY_SECRET_HERE
    

$ kubectl apply -f examples/inference/kaito_workspace_phi_3.yaml
```

Η κατάσταση του workspace μπορεί να παρακολουθηθεί εκτελώντας την παρακάτω εντολή. Όταν η στήλη WORKSPACEREADY γίνει `True`, το μοντέλο έχει αναπτυχθεί επιτυχώς.

```sh
$ kubectl get workspace kaito_workspace_phi_3.yaml
NAME                  INSTANCE            RESOURCEREADY   INFERENCEREADY   WORKSPACEREADY   AGE
workspace-phi-3-mini   Standard_NC6s_v3   True            True             True             10m
```

Στη συνέχεια, μπορείτε να βρείτε το cluster ip της υπηρεσίας inference και να χρησιμοποιήσετε ένα προσωρινό pod `curl` για να δοκιμάσετε το endpoint της υπηρεσίας μέσα στο cluster.

```sh
$ kubectl get svc workspace-phi-3-mini
NAME                  TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)            AGE
workspace-phi-3-mini-adapter  ClusterIP   <CLUSTERIP>  <none>        80/TCP,29500/TCP   10m

export CLUSTERIP=$(kubectl get svc workspace-phi-3-mini-adapter -o jsonpath="{.spec.clusterIPs[0]}") 
$ kubectl run -it --rm --restart=Never curl --image=curlimages/curl -- curl -X POST http://$CLUSTERIP/chat -H "accept: application/json" -H "Content-Type: application/json" -d "{\"prompt\":\"YOUR QUESTION HERE\"}"
```

## Γρήγορη εκκίνηση Inference Phi-3 με adapters

Μετά την εγκατάσταση του Kaito, μπορείτε να δοκιμάσετε τις παρακάτω εντολές για να ξεκινήσετε μια υπηρεσία inference.

[Παράδειγμα κώδικα Inference Phi-3 με Adapters](https://github.com/Azure/kaito/blob/main/examples/inference/kaito_workspace_phi_3_with_adapters.yaml)

```
apiVersion: kaito.sh/v1alpha1
kind: Workspace
metadata:
  name: workspace-phi-3-mini-adapter
resource:
  instanceType: "Standard_NC6s_v3"
  labelSelector:
    matchLabels:
      apps: phi-3-adapter
inference:
  preset:
    name: phi-3-mini-128k-instruct
  adapters:
    - source:
        name: "phi-3-adapter"
        image: "ACR_REPO_HERE.azurecr.io/ADAPTER_HERE:0.0.1"
      strength: "1.0"
```

```sh
$ cat examples/inference/kaito_workspace_phi_3_with_adapters.yaml

apiVersion: kaito.sh/v1alpha1
kind: Workspace
metadata:
  name: workspace-phi-3-mini-adapter
resource:
  instanceType: "Standard_NC6s_v3"
  labelSelector:
    matchLabels:
      app: phi-3-adapter
tuning:
  preset:
    name: phi-3-mini-128k-instruct
  method: qlora
  input:
    urls:
      - "https://huggingface.co/datasets/philschmid/dolly-15k-oai-style/resolve/main/data/train-00000-of-00001-54e3756291ca09c6.parquet?download=true"
  output:
    image: "ACR_REPO_HERE.azurecr.io/IMAGE_NAME_HERE:0.0.1" # Tuning Output ACR Path
    imagePushSecret: ACR_REGISTRY_SECRET_HERE
    

$ kubectl apply -f examples/inference/kaito_workspace_phi_3_with_adapters.yaml
```

Η κατάσταση του workspace μπορεί να παρακολουθηθεί εκτελώντας την παρακάτω εντολή. Όταν η στήλη WORKSPACEREADY γίνει `True`, το μοντέλο έχει αναπτυχθεί επιτυχώς.

```sh
$ kubectl get workspace kaito_workspace_phi_3_with_adapters.yaml
NAME                  INSTANCE            RESOURCEREADY   INFERENCEREADY   WORKSPACEREADY   AGE
workspace-phi-3-mini-adapter   Standard_NC6s_v3   True            True             True             10m
```

Στη συνέχεια, μπορείτε να βρείτε το cluster ip της υπηρεσίας inference και να χρησιμοποιήσετε ένα προσωρινό pod `curl` για να δοκιμάσετε το endpoint της υπηρεσίας μέσα στο cluster.

```sh
$ kubectl get svc workspace-phi-3-mini-adapter
NAME                  TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)            AGE
workspace-phi-3-mini-adapter  ClusterIP   <CLUSTERIP>  <none>        80/TCP,29500/TCP   10m

export CLUSTERIP=$(kubectl get svc workspace-phi-3-mini-adapter -o jsonpath="{.spec.clusterIPs[0]}") 
$ kubectl run -it --rm --restart=Never curl --image=curlimages/curl -- curl -X POST http://$CLUSTERIP/chat -H "accept: application/json" -H "Content-Type: application/json" -d "{\"prompt\":\"YOUR QUESTION HERE\"}"
```

**Αποποίηση ευθυνών**:  
Αυτό το έγγραφο έχει μεταφραστεί χρησιμοποιώντας την υπηρεσία αυτόματης μετάφρασης AI [Co-op Translator](https://github.com/Azure/co-op-translator). Παρόλο που επιδιώκουμε την ακρίβεια, παρακαλούμε να έχετε υπόψη ότι οι αυτόματες μεταφράσεις ενδέχεται να περιέχουν λάθη ή ανακρίβειες. Το πρωτότυπο έγγραφο στη γλώσσα του θεωρείται η αυθεντική πηγή. Για κρίσιμες πληροφορίες, συνιστάται επαγγελματική ανθρώπινη μετάφραση. Δεν φέρουμε ευθύνη για τυχόν παρεξηγήσεις ή λανθασμένες ερμηνείες που προκύπτουν από τη χρήση αυτής της μετάφρασης.
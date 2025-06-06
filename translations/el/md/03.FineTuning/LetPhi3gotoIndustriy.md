<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "743d7e9cb9c4e8ea642d77bee657a7fa",
  "translation_date": "2025-05-09T22:26:47+00:00",
  "source_file": "md/03.FineTuning/LetPhi3gotoIndustriy.md",
  "language_code": "el"
}
-->
# **Αφήστε το Phi-3 να γίνει ειδικός του κλάδου**

Για να ενσωματώσετε το μοντέλο Phi-3 σε έναν κλάδο, πρέπει να προσθέσετε επιχειρηματικά δεδομένα του κλάδου στο μοντέλο Phi-3. Έχουμε δύο διαφορετικές επιλογές, η πρώτη είναι RAG (Retrieval Augmented Generation) και η δεύτερη είναι Fine Tuning.

## **RAG vs Fine-Tuning**

### **Retrieval Augmented Generation**

Το RAG είναι ανάκτηση δεδομένων + δημιουργία κειμένου. Τα δομημένα και αδόμητα δεδομένα της επιχείρησης αποθηκεύονται στη βάση δεδομένων διανυσμάτων. Κατά την αναζήτηση σχετικού περιεχομένου, βρίσκονται οι σχετικές περιλήψεις και το περιεχόμενο για να σχηματίσουν ένα πλαίσιο, και η δυνατότητα συμπλήρωσης κειμένου του LLM/SLM συνδυάζεται για τη δημιουργία περιεχομένου.

### **Fine-tuning**

Το Fine-tuning βασίζεται στη βελτίωση ενός συγκεκριμένου μοντέλου. Δεν χρειάζεται να ξεκινήσει από τον αλγόριθμο του μοντέλου, αλλά τα δεδομένα πρέπει να συσσωρεύονται συνεχώς. Αν θέλετε πιο ακριβή ορολογία και γλωσσική έκφραση στις εφαρμογές του κλάδου, το fine-tuning είναι η καλύτερη επιλογή. Ωστόσο, αν τα δεδομένα σας αλλάζουν συχνά, το fine-tuning μπορεί να γίνει περίπλοκο.

### **Πώς να επιλέξετε**

1. Αν η απάντησή μας απαιτεί εισαγωγή εξωτερικών δεδομένων, το RAG είναι η καλύτερη επιλογή

2. Αν χρειάζεστε σταθερή και ακριβή γνώση του κλάδου, το fine-tuning θα είναι μια καλή επιλογή. Το RAG δίνει προτεραιότητα στην ανάκτηση σχετικού περιεχομένου, αλλά μπορεί να μην αποτυπώνει πάντα τις εξειδικευμένες αποχρώσεις.

3. Το fine-tuning απαιτεί ένα υψηλής ποιότητας σύνολο δεδομένων, και αν είναι μόνο σε μικρό εύρος δεδομένων, δεν θα κάνει μεγάλη διαφορά. Το RAG είναι πιο ευέλικτο

4. Το fine-tuning είναι ένα μαύρο κουτί, μια μεταφυσική διαδικασία, και είναι δύσκολο να κατανοηθεί ο εσωτερικός μηχανισμός. Αντίθετα, το RAG μπορεί να διευκολύνει την εύρεση της πηγής των δεδομένων, επιτρέποντας έτσι την αποτελεσματική διόρθωση παραισθήσεων ή σφαλμάτων περιεχομένου και παρέχοντας καλύτερη διαφάνεια.

### **Σενάρια**

1. Οι κάθετοι κλάδοι απαιτούν συγκεκριμένο επαγγελματικό λεξιλόγιο και εκφράσεις, ***Fine-tuning*** είναι η καλύτερη επιλογή

2. Σύστημα QA, που περιλαμβάνει σύνθεση διαφορετικών σημείων γνώσης, ***RAG*** είναι η καλύτερη επιλογή

3. Ο συνδυασμός αυτοματοποιημένης ροής εργασίας ***RAG + Fine-tuning*** είναι η καλύτερη επιλογή

## **Πώς να χρησιμοποιήσετε το RAG**

![rag](../../../../translated_images/rag.36e7cb856f120334d577fde60c6a5d7c5eecae255dac387669303d30b4b3efa4.el.png)

Μια βάση δεδομένων διανυσμάτων είναι μια συλλογή δεδομένων αποθηκευμένων σε μαθηματική μορφή. Οι βάσεις δεδομένων διανυσμάτων διευκολύνουν τα μοντέλα μηχανικής μάθησης να θυμούνται προηγούμενες εισόδους, επιτρέποντας τη χρήση της μηχανικής μάθησης για την υποστήριξη περιπτώσεων χρήσης όπως αναζήτηση, προτάσεις και δημιουργία κειμένου. Τα δεδομένα μπορούν να αναγνωριστούν με βάση μετρικές ομοιότητας αντί για ακριβείς αντιστοιχίσεις, επιτρέποντας στα υπολογιστικά μοντέλα να κατανοήσουν το πλαίσιο των δεδομένων.

Η βάση δεδομένων διανυσμάτων είναι το κλειδί για την υλοποίηση του RAG. Μπορούμε να μετατρέψουμε τα δεδομένα σε αποθήκευση διανυσμάτων μέσω μοντέλων διανυσμάτων όπως text-embedding-3, jina-ai-embedding κ.ά.

Μάθετε περισσότερα για τη δημιουργία εφαρμογής RAG [https://github.com/microsoft/Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?WT.mc_id=aiml-138114-kinfeylo) 

## **Πώς να χρησιμοποιήσετε το Fine-tuning**

Οι συνήθεις αλγόριθμοι στο Fine-tuning είναι οι Lora και QLora. Πώς να επιλέξετε;
- [Μάθετε περισσότερα με αυτό το δείγμα notebook](../../../../code/04.Finetuning/Phi_3_Inference_Finetuning.ipynb)
- [Παράδειγμα Python FineTuning Sample](../../../../code/04.Finetuning/FineTrainingScript.py)

### **Lora και QLora**

![lora](../../../../translated_images/qlora.6aeba71122bc0c8d56ccf0bc36b861304939fee087f43c1fc6cc5c9cb8764725.el.png)

Το LoRA (Low-Rank Adaptation) και το QLoRA (Quantized Low-Rank Adaptation) είναι τεχνικές που χρησιμοποιούνται για fine-tuning μεγάλων γλωσσικών μοντέλων (LLMs) με τη μέθοδο Parameter Efficient Fine Tuning (PEFT). Οι τεχνικές PEFT έχουν σχεδιαστεί για να εκπαιδεύουν μοντέλα πιο αποδοτικά από τις παραδοσιακές μεθόδους.  
Το LoRA είναι μια ανεξάρτητη τεχνική fine-tuning που μειώνει τη μνήμη εφαρμόζοντας μια προσέγγιση χαμηλής τάξης στον πίνακα ενημέρωσης βαρών. Προσφέρει γρήγορους χρόνους εκπαίδευσης και διατηρεί απόδοση κοντά στις παραδοσιακές μεθόδους fine-tuning.

Το QLoRA είναι μια επεκταμένη έκδοση του LoRA που ενσωματώνει τεχνικές κβαντοποίησης για περαιτέρω μείωση της χρήσης μνήμης. Το QLoRA κβαντοποιεί την ακρίβεια των παραμέτρων βάρους στο προεκπαιδευμένο LLM σε ακρίβεια 4-bit, που είναι πιο αποδοτική στη μνήμη από το LoRA. Ωστόσο, η εκπαίδευση QLoRA είναι περίπου 30% πιο αργή από την εκπαίδευση LoRA λόγω των επιπλέον βημάτων κβαντοποίησης και αποκβαντοποίησης.

Το QLoRA χρησιμοποιεί το LoRA ως βοηθητικό για να διορθώσει τα σφάλματα που προκύπτουν κατά την κβαντοποίηση. Το QLoRA επιτρέπει το fine-tuning τεράστιων μοντέλων με δισεκατομμύρια παραμέτρους σε σχετικά μικρές, ευρέως διαθέσιμες GPUs. Για παράδειγμα, το QLoRA μπορεί να κάνει fine-tuning σε ένα μοντέλο 70B παραμέτρων που απαιτεί 36 GPUs με μόνο 2

**Αποποίηση ευθυνών**:  
Αυτό το έγγραφο έχει μεταφραστεί χρησιμοποιώντας την υπηρεσία αυτόματης μετάφρασης AI [Co-op Translator](https://github.com/Azure/co-op-translator). Παρόλο που προσπαθούμε για ακρίβεια, παρακαλούμε να έχετε υπόψη ότι οι αυτοματοποιημένες μεταφράσεις ενδέχεται να περιέχουν σφάλματα ή ανακρίβειες. Το πρωτότυπο έγγραφο στη γλώσσα του πρέπει να θεωρείται η επίσημη πηγή. Για κρίσιμες πληροφορίες, συνιστάται επαγγελματική μετάφραση από ανθρώπους. Δεν φέρουμε ευθύνη για τυχόν παρεξηγήσεις ή λανθασμένες ερμηνείες που προκύπτουν από τη χρήση αυτής της μετάφρασης.
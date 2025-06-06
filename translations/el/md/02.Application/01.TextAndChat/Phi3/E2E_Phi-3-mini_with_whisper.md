<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "006e8cf75211d3297f24e1b22e38955f",
  "translation_date": "2025-05-09T18:31:09+00:00",
  "source_file": "md/02.Application/01.TextAndChat/Phi3/E2E_Phi-3-mini_with_whisper.md",
  "language_code": "el"
}
-->
# Interactive Phi 3 Mini 4K Instruct Chatbot with Whisper

## Επισκόπηση

Το Interactive Phi 3 Mini 4K Instruct Chatbot είναι ένα εργαλείο που επιτρέπει στους χρήστες να αλληλεπιδρούν με το Microsoft Phi 3 Mini 4K instruct demo χρησιμοποιώντας κείμενο ή φωνητική είσοδο. Το chatbot μπορεί να χρησιμοποιηθεί για διάφορες εργασίες, όπως μετάφραση, ενημερώσεις καιρού και γενική συλλογή πληροφοριών.

### Ξεκινώντας

Για να χρησιμοποιήσετε αυτό το chatbot, απλώς ακολουθήστε αυτές τις οδηγίες:

1. Ανοίξτε ένα νέο [E2E_Phi-3-mini-4k-instruct-Whispser_Demo.ipynb](https://github.com/microsoft/Phi-3CookBook/blob/main/code/06.E2E/E2E_Phi-3-mini-4k-instruct-Whispser_Demo.ipynb)
2. Στο κύριο παράθυρο του notebook, θα δείτε ένα περιβάλλον συνομιλίας με πλαίσιο εισαγωγής κειμένου και κουμπί "Send".
3. Για να χρησιμοποιήσετε το κειμενικό chatbot, απλά πληκτρολογήστε το μήνυμά σας στο πλαίσιο κειμένου και πατήστε το κουμπί "Send". Το chatbot θα απαντήσει με ένα αρχείο ήχου που μπορεί να αναπαραχθεί απευθείας μέσα στο notebook.

**Note**: Αυτό το εργαλείο απαιτεί GPU και πρόσβαση στα μοντέλα Microsoft Phi-3 και OpenAI Whisper, που χρησιμοποιούνται για αναγνώριση ομιλίας και μετάφραση.

### Απαιτήσεις GPU

Για να τρέξετε αυτό το demo χρειάζεστε 12GB μνήμης GPU.

Οι απαιτήσεις μνήμης για το **Microsoft-Phi-3-Mini-4K instruct** demo σε GPU εξαρτώνται από διάφορους παράγοντες, όπως το μέγεθος των δεδομένων εισόδου (ήχος ή κείμενο), η γλώσσα μετάφρασης, η ταχύτητα του μοντέλου και η διαθέσιμη μνήμη στην GPU.

Γενικά, το μοντέλο Whisper έχει σχεδιαστεί για να τρέχει σε GPUs. Η προτεινόμενη ελάχιστη μνήμη GPU για το Whisper είναι 8 GB, αλλά μπορεί να διαχειριστεί και μεγαλύτερα μεγέθη μνήμης αν χρειαστεί.

Είναι σημαντικό να σημειωθεί ότι η εκτέλεση μεγάλου όγκου δεδομένων ή πολλών αιτημάτων μπορεί να απαιτήσει περισσότερη μνήμη GPU και/ή να προκαλέσει θέματα απόδοσης. Συνιστάται να δοκιμάσετε την περίπτωση χρήσης σας με διαφορετικές ρυθμίσεις και να παρακολουθείτε τη χρήση μνήμης για να βρείτε τις βέλτιστες ρυθμίσεις για τις ανάγκες σας.

## Παράδειγμα E2E για το Interactive Phi 3 Mini 4K Instruct Chatbot με Whisper

Το jupyter notebook με τίτλο [Interactive Phi 3 Mini 4K Instruct Chatbot with Whisper](https://github.com/microsoft/Phi-3CookBook/blob/main/code/06.E2E/E2E_Phi-3-mini-4k-instruct-Whispser_Demo.ipynb) δείχνει πώς να χρησιμοποιήσετε το Microsoft Phi 3 Mini 4K instruct Demo για να δημιουργήσετε κείμενο από ήχο ή γραπτή είσοδο. Το notebook ορίζει αρκετές συναρτήσεις:

1. `tts_file_name(text)`: Αυτή η συνάρτηση δημιουργεί όνομα αρχείου βάσει του κειμένου εισόδου για την αποθήκευση του παραγόμενου αρχείου ήχου.
1. `edge_free_tts(chunks_list,speed,voice_name,save_path)`: Αυτή η συνάρτηση χρησιμοποιεί το Edge TTS API για να δημιουργήσει αρχείο ήχου από λίστα τμημάτων κειμένου. Οι παράμετροι εισόδου είναι η λίστα τμημάτων, ο ρυθμός ομιλίας, το όνομα φωνής και η διαδρομή εξόδου για αποθήκευση του αρχείου.
1. `talk(input_text)`: Αυτή η συνάρτηση δημιουργεί αρχείο ήχου χρησιμοποιώντας το Edge TTS API και το αποθηκεύει με τυχαίο όνομα στο φάκελο /content/audio. Η παράμετρος εισόδου είναι το κείμενο προς μετατροπή σε ομιλία.
1. `run_text_prompt(message, chat_history)`: Αυτή η συνάρτηση χρησιμοποιεί το Microsoft Phi 3 Mini 4K instruct demo για να δημιουργήσει αρχείο ήχου από μήνυμα εισόδου και το προσθέτει στο ιστορικό συνομιλίας.
1. `run_audio_prompt(audio, chat_history)`: Αυτή η συνάρτηση μετατρέπει αρχείο ήχου σε κείμενο χρησιμοποιώντας το Whisper μοντέλο μέσω API και το περνάει στη συνάρτηση `run_text_prompt()`.
1. Ο κώδικας εκκινεί μια εφαρμογή Gradio που επιτρέπει στους χρήστες να αλληλεπιδρούν με το Phi 3 Mini 4K instruct demo είτε πληκτρολογώντας μηνύματα είτε ανεβάζοντας αρχεία ήχου. Η έξοδος εμφανίζεται ως μήνυμα κειμένου μέσα στην εφαρμογή.

## Επίλυση Προβλημάτων

Εγκατάσταση Cuda GPU drivers

1. Βεβαιωθείτε ότι οι εφαρμογές Linux είναι ενημερωμένες

    ```bash
    sudo apt update
    ```

1. Εγκαταστήστε τους Cuda Drivers

    ```bash
    sudo apt install nvidia-cuda-toolkit
    ```

1. Καταχωρίστε τη θέση των cuda drivers

    ```bash
    echo /usr/lib64-nvidia/ >/etc/ld.so.conf.d/libcuda.conf; ldconfig
    ```

1. Έλεγχος μεγέθους μνήμης Nvidia GPU (Απαιτούνται 12GB μνήμης GPU)

    ```bash
    nvidia-smi
    ```

1. Άδειασμα Cache: Αν χρησιμοποιείτε PyTorch, μπορείτε να καλέσετε torch.cuda.empty_cache() για να απελευθερώσετε όλη την αχρησιμοποίητη cache μνήμη ώστε να χρησιμοποιηθεί από άλλες εφαρμογές GPU

    ```python
    torch.cuda.empty_cache() 
    ```

1. Έλεγχος Nvidia Cuda

    ```bash
    nvcc --version
    ```

1. Εκτελέστε τα παρακάτω βήματα για να δημιουργήσετε token Hugging Face.

    - Μεταβείτε στη σελίδα [Hugging Face Token Settings](https://huggingface.co/settings/tokens?WT.mc_id=aiml-137032-kinfeylo).
    - Επιλέξτε **New token**.
    - Εισάγετε το όνομα του έργου (**Name**) που θέλετε να χρησιμοποιήσετε.
    - Επιλέξτε **Type** ως **Write**.

> **Note**
>
> Αν συναντήσετε το παρακάτω σφάλμα:
>
> ```bash
> /sbin/ldconfig.real: Can't create temporary cache file /etc/ld.so.cache~: Permission denied 
> ```
>
> Για να το διορθώσετε, πληκτρολογήστε την ακόλουθη εντολή στο τερματικό σας.
>
> ```bash
> sudo ldconfig
> ```

**Αποποίηση Ευθυνών**:  
Αυτό το έγγραφο έχει μεταφραστεί χρησιμοποιώντας την υπηρεσία μετάφρασης με τεχνητή νοημοσύνη [Co-op Translator](https://github.com/Azure/co-op-translator). Παρόλο που προσπαθούμε για ακρίβεια, παρακαλούμε να λάβετε υπόψη ότι οι αυτόματες μεταφράσεις ενδέχεται να περιέχουν λάθη ή ανακρίβειες. Το πρωτότυπο έγγραφο στη μητρική του γλώσσα πρέπει να θεωρείται η αυθεντική πηγή. Για κρίσιμες πληροφορίες, συνιστάται επαγγελματική μετάφραση από ανθρώπους. Δεν φέρουμε ευθύνη για τυχόν παρεξηγήσεις ή λανθασμένες ερμηνείες που προκύπτουν από τη χρήση αυτής της μετάφρασης.
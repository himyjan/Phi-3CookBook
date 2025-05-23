<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "455be2b7b9c3390d367d528f8fab2aa0",
  "translation_date": "2025-05-09T17:48:14+00:00",
  "source_file": "md/02.Application/01.TextAndChat/Phi3/E2E_Phi-3-FineTuning_PromptFlow_Integration.md",
  "language_code": "sk"
}
-->
# Doladiť a integrovať vlastné modely Phi-3 s Prompt flow

Tento kompletný (E2E) príklad je založený na návode "[Fine-Tune and Integrate Custom Phi-3 Models with Prompt Flow: Step-by-Step Guide](https://techcommunity.microsoft.com/t5/educator-developer-blog/fine-tune-and-integrate-custom-phi-3-models-with-prompt-flow/ba-p/4178612?WT.mc_id=aiml-137032-kinfeylo)" z Microsoft Tech Community. Predstavuje procesy doladenia, nasadenia a integrácie vlastných modelov Phi-3 s Prompt flow.

## Prehľad

V tomto E2E príklade sa naučíte, ako doladiť model Phi-3 a integrovať ho s Prompt flow. Využitím Azure Machine Learning a Prompt flow vytvoríte pracovný tok na nasadenie a používanie vlastných AI modelov. Tento príklad je rozdelený do troch scénarov:

**Scenár 1: Nastavenie Azure zdrojov a príprava na doladenie**

**Scenár 2: Doladenie modelu Phi-3 a nasadenie v Azure Machine Learning Studio**

**Scenár 3: Integrácia s Prompt flow a komunikácia s vlastným modelom**

Tu je prehľad tohto E2E príkladu.

![Phi-3-FineTuning_PromptFlow_Integration Overview](../../../../../../translated_images/00-01-architecture.dfeb1f15c7d3c8989fb267a05ac83a25485a7230bde037df9d3d92336afc1993.sk.png)

### Obsah

1. **[Scenár 1: Nastavenie Azure zdrojov a príprava na doladenie](../../../../../../md/02.Application/01.TextAndChat/Phi3)**
    - [Vytvorenie Azure Machine Learning Workspace](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Žiadosť o GPU kvóty v Azure Subscription](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Pridanie priradenia rolí](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Nastavenie projektu](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Príprava datasetu na doladenie](../../../../../../md/02.Application/01.TextAndChat/Phi3)

1. **[Scenár 2: Doladenie modelu Phi-3 a nasadenie v Azure Machine Learning Studio](../../../../../../md/02.Application/01.TextAndChat/Phi3)**
    - [Nastavenie Azure CLI](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Doladenie modelu Phi-3](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Nasadenie doladeného modelu](../../../../../../md/02.Application/01.TextAndChat/Phi3)

1. **[Scenár 3: Integrácia s Prompt flow a komunikácia s vlastným modelom](../../../../../../md/02.Application/01.TextAndChat/Phi3)**
    - [Integrácia vlastného modelu Phi-3 s Prompt flow](../../../../../../md/02.Application/01.TextAndChat/Phi3)
    - [Komunikácia s vlastným modelom](../../../../../../md/02.Application/01.TextAndChat/Phi3)

## Scenár 1: Nastavenie Azure zdrojov a príprava na doladenie

### Vytvorenie Azure Machine Learning Workspace

1. Do **vyhľadávacieho panela** v hornej časti portálu zadajte *azure machine learning* a zo zobrazenej ponuky vyberte **Azure Machine Learning**.

    ![Type azure machine learning](../../../../../../translated_images/01-01-type-azml.321cff72d18a51c06dee2db7463868f3ca6619559a5d68b7795d70f4a8b3a683.sk.png)

1. V navigačnom menu vyberte **+ Create**.

1. V navigačnom menu vyberte **New workspace**.

    ![Select new workspace](../../../../../../translated_images/01-02-select-new-workspace.9bd9208488fcf38226fc8d3cefffecb2cb14f414f6d8d982492c1bde8634e24a.sk.png)

1. Vykonajte nasledovné kroky:

    - Vyberte svoju Azure **Subscription**.
    - Vyberte **Resource group**, ktorú chcete použiť (v prípade potreby vytvorte novú).
    - Zadajte **Workspace Name**. Musí byť jedinečný.
    - Vyberte **Region**, ktorý chcete použiť.
    - Vyberte **Storage account**, ktorý chcete použiť (v prípade potreby vytvorte nový).
    - Vyberte **Key vault**, ktorý chcete použiť (v prípade potreby vytvorte nový).
    - Vyberte **Application insights**, ktorý chcete použiť (v prípade potreby vytvorte nový).
    - Vyberte **Container registry**, ktorý chcete použiť (v prípade potreby vytvorte nový).

    ![Fill AZML.](../../../../../../translated_images/01-03-fill-AZML.b2ebbef59952cd17d16b1f82adc252bf7616f8638d451e3c6595ffefe44f2cfa.sk.png)

1. Vyberte **Review + Create**.

1. Vyberte **Create**.

### Žiadosť o GPU kvóty v Azure Subscription

V tomto E2E príklade použijete *Standard_NC24ads_A100_v4 GPU* na doladenie, čo vyžaduje žiadosť o kvótu, a *Standard_E4s_v3* CPU na nasadenie, čo kvótu nevyžaduje.

> [!NOTE]
>
> GPU sú prideľované len pre Pay-As-You-Go subscription (štandardný typ subscription); benefitné subscription zatiaľ nie sú podporované.
>
> Pre používateľov benefitných subscription (napr. Visual Studio Enterprise Subscription) alebo tých, ktorí chcú rýchlo otestovať doladenie a nasadenie, tento návod ponúka aj možnosť doladenia s minimálnym datasetom na CPU. Treba však počítať s tým, že výsledky doladenia sú výrazne lepšie pri použití GPU a väčších datasetov.

1. Navštívte [Azure ML Studio](https://ml.azure.com/home?wt.mc_id=studentamb_279723).

1. Vykonajte nasledujúce kroky na žiadosť o kvótu pre *Standard NCADSA100v4 Family*:

    - Vyberte **Quota** v ľavom menu.
    - Vyberte **Virtual machine family** na použitie, napríklad **Standard NCADSA100v4 Family Cluster Dedicated vCPUs**, ktorá obsahuje *Standard_NC24ads_A100_v4* GPU.
    - Vyberte **Request quota** v navigačnom menu.

        ![Request quota.](../../../../../../translated_images/01-04-request-quota.ddf063c7cda9799b8ef6fbde6c3c796201578fe9078feb1c624ed75c7705ad18.sk.png)

    - Na stránke Request quota zadajte požadovaný limit **New cores limit**, napríklad 24.
    - Na stránke Request quota vyberte **Submit** pre odoslanie žiadosti o GPU kvótu.

> [!NOTE]
> Podľa potreby si môžete vybrať vhodný GPU alebo CPU podľa dokumentu [Sizes for Virtual Machines in Azure](https://learn.microsoft.com/azure/virtual-machines/sizes/overview?tabs=breakdownseries%2Cgeneralsizelist%2Ccomputesizelist%2Cmemorysizelist%2Cstoragesizelist%2Cgpusizelist%2Cfpgasizelist%2Chpcsizelist).

### Pridanie priradenia rolí

Na doladenie a nasadenie modelov musíte najprv vytvoriť User Assigned Managed Identity (UAI) a priradiť jej príslušné povolenia. Táto UAI sa použije na autentifikáciu počas nasadenia.

#### Vytvorenie User Assigned Managed Identity (UAI)

1. Do **vyhľadávacieho panela** v hornej časti portálu zadajte *managed identities* a zo zobrazenej ponuky vyberte **Managed Identities**.

    ![Type managed identities.](../../../../../../translated_images/01-05-type-managed-identities.8bf5dc5a4fa3e852f897ec1a983e506c2bc7b7113d159598bf0adeb66d20a5c4.sk.png)

1. Vyberte **+ Create**.

    ![Select create.](../../../../../../translated_images/01-06-select-create.025632b7b54fe323f7d38edabbae05dd23f4665d0731f7143719c27c32e7e84f.sk.png)

1. Vykonajte nasledujúce kroky:

    - Vyberte svoju Azure **Subscription**.
    - Vyberte **Resource group** na použitie (v prípade potreby vytvorte novú).
    - Vyberte **Region**, ktorý chcete použiť.
    - Zadajte **Name**. Musí byť jedinečný.

1. Vyberte **Review + create**.

1. Vyberte **+ Create**.

#### Pridanie priradenia roly Contributor Managed Identity

1. Prejdite do zdroja Managed Identity, ktorý ste vytvorili.

1. Vyberte **Azure role assignments** v ľavom menu.

1. Vyberte **+Add role assignment** v navigačnom menu.

1. Na stránke Add role assignment vykonajte nasledujúce kroky:
    - Vyberte **Scope** na **Resource group**.
    - Vyberte svoju Azure **Subscription**.
    - Vyberte **Resource group** na použitie.
    - Vyberte rolu **Contributor**.

    ![Fill contributor role.](../../../../../../translated_images/01-07-fill-contributor-role.8936866326c7cdc3b876f14657e03422cca9dbff8b193dd541a693ce34407b26.sk.png)

1. Vyberte **Save**.

#### Pridanie priradenia roly Storage Blob Data Reader Managed Identity

1. Do **vyhľadávacieho panela** v hornej časti portálu zadajte *storage accounts* a zo zobrazenej ponuky vyberte **Storage accounts**.

    ![Type storage accounts.](../../../../../../translated_images/01-08-type-storage-accounts.83554a27ff3edb5099ee3fbf7f467b843dabcc0e0e5fbb829a341eab3469ffa5.sk.png)

1. Vyberte storage account, ktorý je prepojený s Azure Machine Learning workspace, ktorý ste vytvorili. Napríklad *finetunephistorage*.

1. Prejdite nasledovne na stránku Add role assignment:

    - Prejdite do Azure Storage account, ktorý ste vytvorili.
    - Vyberte **Access Control (IAM)** v ľavom menu.
    - Vyberte **+ Add** v navigačnom menu.
    - Vyberte **Add role assignment**.

    ![Add role.](../../../../../../translated_images/01-09-add-role.4fef55886792c7e860da4c5a808044e6f7067fb5694f3ed4819a5758c6cc574e.sk.png)

1. Na stránke Add role assignment vykonajte nasledovné:

    - Do vyhľadávacieho panela na stránke Role zadajte *Storage Blob Data Reader* a vyberte túto rolu.
    - Vyberte **Next**.
    - Na stránke Members vyberte **Assign access to** **Managed identity**.
    - Vyberte **+ Select members**.
    - Na stránke Select managed identities vyberte svoju Azure **Subscription**.
    - Vyberte **Managed identity** ako **Manage Identity**.
    - Vyberte Managed Identity, ktorú ste vytvorili, napríklad *finetunephi-managedidentity*.
    - Vyberte **Select**.

    ![Select managed identity.](../../../../../../translated_images/01-10-select-managed-identity.fffa802e4e6ce2de4fe50e64d37d3f2ef268c2ee16f30ec6f92bd1829b5f19c1.sk.png)

1. Vyberte **Review + assign**.

#### Pridanie priradenia roly AcrPull Managed Identity

1. Do **vyhľadávacieho panela** v hornej časti portálu zadajte *container registries* a zo zobrazenej ponuky vyberte **Container registries**.

    ![Type container registries.](../../../../../../translated_images/01-11-type-container-registries.62e58403d73d16a0cc715571c8a7b4105a0e97b1422aa5f26106aff1c0e8a47d.sk.png)

1. Vyberte container registry, ktorý je prepojený s Azure Machine Learning workspace. Napríklad *finetunephicontainerregistries*.

1. Prejdite nasledovne na stránku Add role assignment:

    - Vyberte **Access Control (IAM)** v ľavom menu.
    - Vyberte **+ Add** v navigačnom menu.
    - Vyberte **Add role assignment**.

1. Na stránke Add role assignment vykonajte nasledovné:

    - Do vyhľadávacieho panela na stránke Role zadajte *AcrPull* a vyberte túto rolu.
    - Vyberte **Next**.
    - Na stránke Members vyberte **Assign access to** **Managed identity**.
    - Vyberte **+ Select members**.
    - Na stránke Select managed identities vyberte svoju Azure **Subscription**.
    - Vyberte **Managed identity** ako **Manage Identity**.
    - Vyberte Managed Identity, ktorú ste vytvorili, napríklad *finetunephi-managedidentity*.
    - Vyberte **Select**.
    - Vyberte **Review + assign**.

### Nastavenie projektu

Teraz vytvoríte priečinok na prácu a nastavíte virtuálne prostredie na vývoj programu, ktorý komunikuje s používateľmi a používa uloženú históriu chatov z Azure Cosmos DB na informovanie svojich odpovedí.

#### Vytvorenie pracovného priečinka

1. Otvorte terminál a zadajte nasledujúci príkaz na vytvorenie priečinka s názvom *finetune-phi* v predvolenej ceste.

    ```console
    mkdir finetune-phi
    ```

1. V termináli zadajte príkaz na presun do priečinka *finetune-phi*, ktorý ste vytvorili.

    ```console
    cd finetune-phi
    ```

#### Vytvorenie virtuálneho prostredia

1. V termináli zadajte príkaz na vytvorenie virtuálneho prostredia s názvom *.venv*.

    ```console
    python -m venv .venv
    ```

1. V termináli zadajte príkaz na aktiváciu virtuálneho prostredia.

    ```console
    .venv\Scripts\activate.bat
    ```

> [!NOTE]
>
> Ak to funguje, mali by ste vidieť *(.venv)* pred príkazovým riadkom.

#### Inštalácia potrebných balíčkov

1. V termináli zadajte nasledujúce príkazy na inštaláciu potrebných balíčkov.

    ```console
    pip install datasets==2.19.1
    pip install transformers==4.41.1
    pip install azure-ai-ml==1.16.0
    pip install torch==2.3.1
    pip install trl==0.9.4
    pip install promptflow==1.12.0
    ```

#### Vytvorenie súborov projektu

V tomto cvičení vytvoríte základné súbory nášho projektu. Tieto súbory obsahujú skripty na stiahnutie datasetu, nastavenie prostredia Azure Machine Learning, doladenie modelu Phi-3 a nasadenie doladeného modelu. Tiež vytvoríte súbor *conda.yml* na nastavenie prostredia pre doladenie.

V tomto cvičení:

- Vytvoríte súbor *download_dataset.py* na stiahnutie datasetu.
- Vytvoríte súbor *setup_ml.py* na nastavenie Azure Machine Learning prostredia.
- V priečinku *finetuning_dir* vytvoríte súbor *fine_tune.py* na doladenie modelu Phi-3 pomocou datasetu.
- Vytvoríte súbor *conda.yml* na nastavenie prostredia pre doladenie.
- Vytvoríte súbor *deploy_model.py* na nasadenie doladeného modelu.
- Vytvoríte súbor *integrate_with_promptflow.py* na integráciu doladeného modelu a jeho spustenie pomocou Prompt flow.
- Vytvoríte súbor *flow.dag.yml* na nastavenie štruktúry pracovného toku pre Prompt flow.
- Vytvoríte súbor *config.py* na zadanie informácií o Azure.

> [!NOTE]
>
> Kompletná štruktúra priečinkov:
>
> ```text
> └── YourUserName
> .    └── finetune-phi
> .        ├── finetuning_dir
> .        │      └── fine_tune.py
> .        ├── conda.yml
> .        ├── config.py
> .        ├── deploy_model.py
> .        ├── download_dataset.py
> .        ├── flow.dag.yml
> .        ├── integrate_with_promptflow.py
> .        └── setup_ml.py
> ```

1. Otvorte **Visual Studio Code**.

1. V menu vyberte **File**.

1. Vyberte **Open Folder**.

1. Vyberte priečinok *finetune-phi*, ktorý ste vytvorili, nachádzajúci sa v *C:\Users\yourUserName\finetune-phi*.

    ![Open project floder.](../../../../../../translated_images/01-12-open-project-folder.1f7f0f79e5d4d62e546e906e1ce5a480cd98d06062ce292b7b99c6cfcd434fdf.sk.png)

1. V ľavom paneli Visual Studio Code kliknite pravým tlačidlom myši a vyberte **New File** na vytvorenie nového súboru s názvom *download_dataset.py*.

1. V ľavom paneli Visual Studio Code kliknite pravým tlačidlom myši a vyberte **New File** na vytvorenie nového súboru s názvom *setup_ml.py*.

1. V ľavom paneli Visual Studio Code kliknite pravým tlačidlom myši a vyberte **New File** na vytvorenie nového súboru s názvom *deploy_model.py*.

    ![Create new file.](../../../../../../translated_images/01-13-create-new-file.40698c2e0415929e7b6dc2b30925677e413f965bac4134d3aefa0b44d443deaf.sk.png)

1. V ľavom paneli Visual Studio Code kliknite pravým tlačidlom myši a vyberte **New Folder** na vytvorenie nového priečinka s názvom *finetuning_dir*.

1. V priečinku *finetuning_dir* vytvorte nový súbor s názvom *fine_tune.py*.

#### Vytvorenie a konfigurácia súboru *conda.yml*

1. V ľavom paneli Visual Studio Code kliknite pravým tlačidlom myši a vyberte **New File** na vytvorenie súboru *conda.yml*.

1. Pridajte nasledujúci kód do súboru *conda.yml* na nastavenie prostredia pre doladenie modelu Phi-3.

    ```yml
    name: phi-3-training-env
    channels:
      - defaults
      - conda-forge
    dependencies:
      - python=3.10
      - pip
      - numpy<2.0
      - pip:
          - torch==2.4.0
          - torchvision==0.19.0
          - trl==0.8.6
          - transformers==4.41
          - datasets==2.21.0
          - azureml-core==1.57.0
          - azure-storage-blob==12.19.0
          - azure-ai-ml==1.16
          - azure-identity==1.17.1
          - accelerate==0.33.0
          - mlflow==2.15.1
          - azureml-mlflow==1.57.0
    ```

#### Vytvorenie a konfigurácia súboru *config.py*

1. V ľavom paneli Visual Studio Code kliknite pravým tlačidlom myši a vyberte **New File** na vytvorenie súboru *config.py*.

1. Pridajte nasledujúci kód do súboru *config.py* na zadanie vašich Azure informácií.

    ```python
    # Azure settings
    AZURE_SUBSCRIPTION_ID = "your_subscription_id"
    AZURE_RESOURCE_GROUP_NAME = "your_resource_group_name" # "TestGroup"

    # Azure Machine Learning settings
    AZURE_ML_WORKSPACE_NAME = "your_workspace_name" # "finetunephi-workspace"

    # Azure Managed Identity settings
    AZURE_MANAGED_IDENTITY_CLIENT_ID = "your_azure_managed_identity_client_id"
    AZURE_MANAGED_IDENTITY_NAME = "your_azure_managed_identity_name" # "finetunephi-mangedidentity"
    AZURE_MANAGED_IDENTITY_RESOURCE_ID = f"/subscriptions/{AZURE_SUBSCRIPTION_ID}/resourceGroups/{AZURE_RESOURCE_GROUP_NAME}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{AZURE_MANAGED_IDENTITY_NAME}"

    # Dataset file paths
    TRAIN_DATA_PATH = "data/train_data.jsonl"
    TEST_DATA_PATH = "data/test_data.jsonl"

    # Fine-tuned model settings
    AZURE_MODEL_NAME = "your_fine_tuned_model_name" # "finetune-phi-model"
    AZURE_ENDPOINT_NAME = "your_fine_tuned_model_endpoint_name" # "finetune-phi-endpoint"
    AZURE_DEPLOYMENT_NAME = "your_fine_tuned_model_deployment_name" # "finetune-phi-deployment"

    AZURE_ML_API_KEY = "your_fine_tuned_model_api_key"
    AZURE_ML_ENDPOINT = "your_fine_tuned_model_endpoint_uri" # "https://{your-endpoint-name}.{your-region}.inference.ml.azure.com/score"
    ```

#### Pridanie Azure environmentálnych premenných

1. Vykonajte nasledujúce kroky na pridanie Azure Subscription ID:

    - Do **vy
![Nájdite ID predplatného.](../../../../../../translated_images/01-14-find-subscriptionid.4daef33360f6d3808a9f1acea2b6b6121c498c75c36cb6ecc6c6b211f0d3b725.sk.png)

1. Vykonajte nasledujúce kroky na pridanie názvu Azure Workspace:

    - Prejdite na zdroj Azure Machine Learning, ktorý ste vytvorili.
    - Skopírujte a vložte názov svojho účtu do súboru *config.py*.

    ![Nájdite názov Azure Machine Learning.](../../../../../../translated_images/01-15-find-AZML-name.c8efdc5a8f2e594260004695c145fafb4fd903e96715f495a43733560cd706b5.sk.png)

1. Vykonajte nasledujúce kroky na pridanie názvu Azure Resource Group:

    - Prejdite na zdroj Azure Machine Learning, ktorý ste vytvorili.
    - Skopírujte a vložte názov svojej Azure Resource Group do súboru *config.py*.

    ![Nájdite názov resource group.](../../../../../../translated_images/01-16-find-AZML-resourcegroup.0647be51d3f1b8183995949df5866455e5532ef1c3d1f93b33dc9a91d615e882.sk.png)

2. Vykonajte nasledujúce kroky na pridanie názvu Azure Managed Identity

    - Prejdite na zdroj Managed Identities, ktorý ste vytvorili.
    - Skopírujte a vložte názov svojej Azure Managed Identity do súboru *config.py*.

    ![Nájdite UAI.](../../../../../../translated_images/01-17-find-uai.b0fe7164cacc93b03c3c534daee68da244de6de4e6dcbc2a4e9df43403eb0f1b.sk.png)

### Pripravte dataset na doladenie

V tomto cvičení spustíte súbor *download_dataset.py*, aby ste stiahli datasety *ULTRACHAT_200k* do svojho lokálneho prostredia. Následne tieto datasety použijete na doladenie modelu Phi-3 v Azure Machine Learning.

#### Stiahnite svoj dataset pomocou *download_dataset.py*

1. Otvorte súbor *download_dataset.py* vo Visual Studio Code.

1. Pridajte nasledujúci kód do *download_dataset.py*.

    ```python
    import json
    import os
    from datasets import load_dataset
    from config import (
        TRAIN_DATA_PATH,
        TEST_DATA_PATH)

    def load_and_split_dataset(dataset_name, config_name, split_ratio):
        """
        Load and split a dataset.
        """
        # Load the dataset with the specified name, configuration, and split ratio
        dataset = load_dataset(dataset_name, config_name, split=split_ratio)
        print(f"Original dataset size: {len(dataset)}")
        
        # Split the dataset into train and test sets (80% train, 20% test)
        split_dataset = dataset.train_test_split(test_size=0.2)
        print(f"Train dataset size: {len(split_dataset['train'])}")
        print(f"Test dataset size: {len(split_dataset['test'])}")
        
        return split_dataset

    def save_dataset_to_jsonl(dataset, filepath):
        """
        Save a dataset to a JSONL file.
        """
        # Create the directory if it does not exist
        os.makedirs(os.path.dirname(filepath), exist_ok=True)
        
        # Open the file in write mode
        with open(filepath, 'w', encoding='utf-8') as f:
            # Iterate over each record in the dataset
            for record in dataset:
                # Dump the record as a JSON object and write it to the file
                json.dump(record, f)
                # Write a newline character to separate records
                f.write('\n')
        
        print(f"Dataset saved to {filepath}")

    def main():
        """
        Main function to load, split, and save the dataset.
        """
        # Load and split the ULTRACHAT_200k dataset with a specific configuration and split ratio
        dataset = load_and_split_dataset("HuggingFaceH4/ultrachat_200k", 'default', 'train_sft[:1%]')
        
        # Extract the train and test datasets from the split
        train_dataset = dataset['train']
        test_dataset = dataset['test']

        # Save the train dataset to a JSONL file
        save_dataset_to_jsonl(train_dataset, TRAIN_DATA_PATH)
        
        # Save the test dataset to a separate JSONL file
        save_dataset_to_jsonl(test_dataset, TEST_DATA_PATH)

    if __name__ == "__main__":
        main()

    ```

> [!TIP]
>
> **Návod na doladenie s minimálnym datasetom pomocou CPU**
>
> Ak chcete použiť CPU na doladenie, tento prístup je ideálny pre tých, ktorí majú benefitové predplatné (napríklad Visual Studio Enterprise Subscription) alebo chcú rýchlo otestovať proces doladenia a nasadenia.
>
> Nahraďte `dataset = load_and_split_dataset("HuggingFaceH4/ultrachat_200k", 'default', 'train_sft[:1%]')` with `dataset = load_and_split_dataset("HuggingFaceH4/ultrachat_200k", 'default', 'train_sft[:10]')`
>

1. Zadajte nasledujúci príkaz v termináli na spustenie skriptu a stiahnutie datasetu do lokálneho prostredia.

    ```console
    python download_data.py
    ```

1. Overte, že datasety boli úspešne uložené do lokálneho priečinka *finetune-phi/data*.

> [!NOTE]
>
> **Veľkosť datasetu a čas doladenia**
>
> V tomto E2E príklade používate len 1 % datasetu (`train_sft[:1%]`). To výrazne znižuje množstvo dát, čím sa zrýchľuje nahrávanie aj samotný proces doladenia. Môžete upraviť percento, aby ste našli správnu rovnováhu medzi časom trénovania a výkonom modelu. Použitie menšej časti datasetu skracuje čas potrebný na doladenie, čo robí proces zvládnuteľnejším pre E2E príklad.

## Scenár 2: Doladenie modelu Phi-3 a nasadenie v Azure Machine Learning Studio

### Nastavenie Azure CLI

Je potrebné nastaviť Azure CLI na autentifikáciu vášho prostredia. Azure CLI umožňuje spravovať Azure zdroje priamo z príkazového riadku a poskytuje prihlasovacie údaje potrebné pre Azure Machine Learning na prístup k týmto zdrojom. Na začiatok nainštalujte [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli)

1. Otvorte terminálové okno a zadajte nasledujúci príkaz na prihlásenie do svojho Azure účtu.

    ```console
    az login
    ```

1. Vyberte svoj Azure účet, ktorý chcete použiť.

1. Vyberte Azure predplatné, ktoré chcete použiť.

    ![Nájdite názov resource group.](../../../../../../translated_images/02-01-login-using-azure-cli.b6e8fb6255e8d09673cb48eca2b12aebbb84dfb8817af8a6b1dfd4bb2759d68f.sk.png)

> [!TIP]
>
> Ak máte problém s prihlásením do Azure, skúste použiť prihlasovanie cez device code. Otvorte terminál a zadajte nasledujúci príkaz na prihlásenie do Azure účtu:
>
> ```console
> az login --use-device-code
> ```
>

### Doladenie modelu Phi-3

V tomto cvičení doladíte model Phi-3 pomocou poskytnutého datasetu. Najskôr definujete proces doladenia v súbore *fine_tune.py*. Potom nakonfigurujete prostredie Azure Machine Learning a spustíte proces doladenia pomocou súboru *setup_ml.py*. Tento skript zabezpečí, že doladenie prebehne v prostredí Azure Machine Learning.

Spustením *setup_ml.py* spustíte proces doladenia v prostredí Azure Machine Learning.

#### Pridajte kód do súboru *fine_tune.py*

1. Prejdite do priečinka *finetuning_dir* a otvorte súbor *fine_tune.py* vo Visual Studio Code.

1. Pridajte nasledujúci kód do *fine_tune.py*.

    ```python
    import argparse
    import sys
    import logging
    import os
    from datasets import load_dataset
    import torch
    import mlflow
    from transformers import AutoModelForCausalLM, AutoTokenizer, TrainingArguments
    from trl import SFTTrainer

    # To avoid the INVALID_PARAMETER_VALUE error in MLflow, disable MLflow integration
    os.environ["DISABLE_MLFLOW_INTEGRATION"] = "True"

    # Logging setup
    logging.basicConfig(
        format="%(asctime)s - %(levelname)s - %(name)s - %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        handlers=[logging.StreamHandler(sys.stdout)],
        level=logging.WARNING
    )
    logger = logging.getLogger(__name__)

    def initialize_model_and_tokenizer(model_name, model_kwargs):
        """
        Initialize the model and tokenizer with the given pretrained model name and arguments.
        """
        model = AutoModelForCausalLM.from_pretrained(model_name, **model_kwargs)
        tokenizer = AutoTokenizer.from_pretrained(model_name)
        tokenizer.model_max_length = 2048
        tokenizer.pad_token = tokenizer.unk_token
        tokenizer.pad_token_id = tokenizer.convert_tokens_to_ids(tokenizer.pad_token)
        tokenizer.padding_side = 'right'
        return model, tokenizer

    def apply_chat_template(example, tokenizer):
        """
        Apply a chat template to tokenize messages in the example.
        """
        messages = example["messages"]
        if messages[0]["role"] != "system":
            messages.insert(0, {"role": "system", "content": ""})
        example["text"] = tokenizer.apply_chat_template(
            messages, tokenize=False, add_generation_prompt=False
        )
        return example

    def load_and_preprocess_data(train_filepath, test_filepath, tokenizer):
        """
        Load and preprocess the dataset.
        """
        train_dataset = load_dataset('json', data_files=train_filepath, split='train')
        test_dataset = load_dataset('json', data_files=test_filepath, split='train')
        column_names = list(train_dataset.features)

        train_dataset = train_dataset.map(
            apply_chat_template,
            fn_kwargs={"tokenizer": tokenizer},
            num_proc=10,
            remove_columns=column_names,
            desc="Applying chat template to train dataset",
        )

        test_dataset = test_dataset.map(
            apply_chat_template,
            fn_kwargs={"tokenizer": tokenizer},
            num_proc=10,
            remove_columns=column_names,
            desc="Applying chat template to test dataset",
        )

        return train_dataset, test_dataset

    def train_and_evaluate_model(train_dataset, test_dataset, model, tokenizer, output_dir):
        """
        Train and evaluate the model.
        """
        training_args = TrainingArguments(
            bf16=True,
            do_eval=True,
            output_dir=output_dir,
            eval_strategy="epoch",
            learning_rate=5.0e-06,
            logging_steps=20,
            lr_scheduler_type="cosine",
            num_train_epochs=3,
            overwrite_output_dir=True,
            per_device_eval_batch_size=4,
            per_device_train_batch_size=4,
            remove_unused_columns=True,
            save_steps=500,
            seed=0,
            gradient_checkpointing=True,
            gradient_accumulation_steps=1,
            warmup_ratio=0.2,
        )

        trainer = SFTTrainer(
            model=model,
            args=training_args,
            train_dataset=train_dataset,
            eval_dataset=test_dataset,
            max_seq_length=2048,
            dataset_text_field="text",
            tokenizer=tokenizer,
            packing=True
        )

        train_result = trainer.train()
        trainer.log_metrics("train", train_result.metrics)

        mlflow.transformers.log_model(
            transformers_model={"model": trainer.model, "tokenizer": tokenizer},
            artifact_path=output_dir,
        )

        tokenizer.padding_side = 'left'
        eval_metrics = trainer.evaluate()
        eval_metrics["eval_samples"] = len(test_dataset)
        trainer.log_metrics("eval", eval_metrics)

    def main(train_file, eval_file, model_output_dir):
        """
        Main function to fine-tune the model.
        """
        model_kwargs = {
            "use_cache": False,
            "trust_remote_code": True,
            "torch_dtype": torch.bfloat16,
            "device_map": None,
            "attn_implementation": "eager"
        }

        # pretrained_model_name = "microsoft/Phi-3-mini-4k-instruct"
        pretrained_model_name = "microsoft/Phi-3.5-mini-instruct"

        with mlflow.start_run():
            model, tokenizer = initialize_model_and_tokenizer(pretrained_model_name, model_kwargs)
            train_dataset, test_dataset = load_and_preprocess_data(train_file, eval_file, tokenizer)
            train_and_evaluate_model(train_dataset, test_dataset, model, tokenizer, model_output_dir)

    if __name__ == "__main__":
        parser = argparse.ArgumentParser()
        parser.add_argument("--train-file", type=str, required=True, help="Path to the training data")
        parser.add_argument("--eval-file", type=str, required=True, help="Path to the evaluation data")
        parser.add_argument("--model_output_dir", type=str, required=True, help="Directory to save the fine-tuned model")
        args = parser.parse_args()
        main(args.train_file, args.eval_file, args.model_output_dir)

    ```

1. Uložte a zatvorte súbor *fine_tune.py*.

> [!TIP]
> **Doladiť môžete aj model Phi-3.5**
>
> V súbore *fine_tune.py* môžete zmeniť hodnotu `pretrained_model_name` from `"microsoft/Phi-3-mini-4k-instruct"` to any model you want to fine-tune. For example, if you change it to `"microsoft/Phi-3.5-mini-instruct"`, you'll be using the Phi-3.5-mini-instruct model for fine-tuning. To find and use the model name you prefer, visit [Hugging Face](https://huggingface.co/), search for the model you're interested in, and then copy and paste its name into the `pretrained_model_name` vo vašom skripte.
>
> :::image type="content" source="../../imgs/03/FineTuning-PromptFlow/finetunephi3.5.png" alt-text="Doladenie Phi-3.5.":::
>

#### Pridajte kód do súboru *setup_ml.py*

1. Otvorte súbor *setup_ml.py* vo Visual Studio Code.

1. Pridajte nasledujúci kód do *setup_ml.py*.

    ```python
    import logging
    from azure.ai.ml import MLClient, command, Input
    from azure.ai.ml.entities import Environment, AmlCompute
    from azure.identity import AzureCliCredential
    from config import (
        AZURE_SUBSCRIPTION_ID,
        AZURE_RESOURCE_GROUP_NAME,
        AZURE_ML_WORKSPACE_NAME,
        TRAIN_DATA_PATH,
        TEST_DATA_PATH
    )

    # Constants

    # Uncomment the following lines to use a CPU instance for training
    # COMPUTE_INSTANCE_TYPE = "Standard_E16s_v3" # cpu
    # COMPUTE_NAME = "cpu-e16s-v3"
    # DOCKER_IMAGE_NAME = "mcr.microsoft.com/azureml/openmpi4.1.0-ubuntu20.04:latest"

    # Uncomment the following lines to use a GPU instance for training
    COMPUTE_INSTANCE_TYPE = "Standard_NC24ads_A100_v4"
    COMPUTE_NAME = "gpu-nc24s-a100-v4"
    DOCKER_IMAGE_NAME = "mcr.microsoft.com/azureml/curated/acft-hf-nlp-gpu:59"

    CONDA_FILE = "conda.yml"
    LOCATION = "eastus2" # Replace with the location of your compute cluster
    FINETUNING_DIR = "./finetuning_dir" # Path to the fine-tuning script
    TRAINING_ENV_NAME = "phi-3-training-environment" # Name of the training environment
    MODEL_OUTPUT_DIR = "./model_output" # Path to the model output directory in azure ml

    # Logging setup to track the process
    logger = logging.getLogger(__name__)
    logging.basicConfig(
        format="%(asctime)s - %(levelname)s - %(name)s - %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        level=logging.WARNING
    )

    def get_ml_client():
        """
        Initialize the ML Client using Azure CLI credentials.
        """
        credential = AzureCliCredential()
        return MLClient(credential, AZURE_SUBSCRIPTION_ID, AZURE_RESOURCE_GROUP_NAME, AZURE_ML_WORKSPACE_NAME)

    def create_or_get_environment(ml_client):
        """
        Create or update the training environment in Azure ML.
        """
        env = Environment(
            image=DOCKER_IMAGE_NAME,  # Docker image for the environment
            conda_file=CONDA_FILE,  # Conda environment file
            name=TRAINING_ENV_NAME,  # Name of the environment
        )
        return ml_client.environments.create_or_update(env)

    def create_or_get_compute_cluster(ml_client, compute_name, COMPUTE_INSTANCE_TYPE, location):
        """
        Create or update the compute cluster in Azure ML.
        """
        try:
            compute_cluster = ml_client.compute.get(compute_name)
            logger.info(f"Compute cluster '{compute_name}' already exists. Reusing it for the current run.")
        except Exception:
            logger.info(f"Compute cluster '{compute_name}' does not exist. Creating a new one with size {COMPUTE_INSTANCE_TYPE}.")
            compute_cluster = AmlCompute(
                name=compute_name,
                size=COMPUTE_INSTANCE_TYPE,
                location=location,
                tier="Dedicated",  # Tier of the compute cluster
                min_instances=0,  # Minimum number of instances
                max_instances=1  # Maximum number of instances
            )
            ml_client.compute.begin_create_or_update(compute_cluster).wait()  # Wait for the cluster to be created
        return compute_cluster

    def create_fine_tuning_job(env, compute_name):
        """
        Set up the fine-tuning job in Azure ML.
        """
        return command(
            code=FINETUNING_DIR,  # Path to fine_tune.py
            command=(
                "python fine_tune.py "
                "--train-file ${{inputs.train_file}} "
                "--eval-file ${{inputs.eval_file}} "
                "--model_output_dir ${{inputs.model_output}}"
            ),
            environment=env,  # Training environment
            compute=compute_name,  # Compute cluster to use
            inputs={
                "train_file": Input(type="uri_file", path=TRAIN_DATA_PATH),  # Path to the training data file
                "eval_file": Input(type="uri_file", path=TEST_DATA_PATH),  # Path to the evaluation data file
                "model_output": MODEL_OUTPUT_DIR
            }
        )

    def main():
        """
        Main function to set up and run the fine-tuning job in Azure ML.
        """
        # Initialize ML Client
        ml_client = get_ml_client()

        # Create Environment
        env = create_or_get_environment(ml_client)
        
        # Create or get existing compute cluster
        create_or_get_compute_cluster(ml_client, COMPUTE_NAME, COMPUTE_INSTANCE_TYPE, LOCATION)

        # Create and Submit Fine-Tuning Job
        job = create_fine_tuning_job(env, COMPUTE_NAME)
        returned_job = ml_client.jobs.create_or_update(job)  # Submit the job
        ml_client.jobs.stream(returned_job.name)  # Stream the job logs
        
        # Capture the job name
        job_name = returned_job.name
        print(f"Job name: {job_name}")

    if __name__ == "__main__":
        main()

    ```

1. Nahraďte `COMPUTE_INSTANCE_TYPE`, `COMPUTE_NAME`, and `LOCATION` vašimi konkrétnymi údajmi.

    ```python
   # Uncomment the following lines to use a GPU instance for training
    COMPUTE_INSTANCE_TYPE = "Standard_NC24ads_A100_v4"
    COMPUTE_NAME = "gpu-nc24s-a100-v4"
    ...
    LOCATION = "eastus2" # Replace with the location of your compute cluster
    ```

> [!TIP]
>
> **Návod na doladenie s minimálnym datasetom pomocou CPU**
>
> Ak chcete použiť CPU na doladenie, tento prístup je ideálny pre tých, ktorí majú benefitové predplatné (napríklad Visual Studio Enterprise Subscription) alebo chcú rýchlo otestovať proces doladenia a nasadenia.
>
> 1. Otvorte súbor *setup_ml*.
> 1. Nahraďte `COMPUTE_INSTANCE_TYPE`, `COMPUTE_NAME`, and `DOCKER_IMAGE_NAME` with the following. If you do not have access to *Standard_E16s_v3*, you can use an equivalent CPU instance or request a new quota.
> 1. Replace `LOCATION` vašimi konkrétnymi údajmi.
>
>    ```python
>    # Uncomment the following lines to use a CPU instance for training
>    COMPUTE_INSTANCE_TYPE = "Standard_E16s_v3" # cpu
>    COMPUTE_NAME = "cpu-e16s-v3"
>    DOCKER_IMAGE_NAME = "mcr.microsoft.com/azureml/openmpi4.1.0-ubuntu20.04:latest"
>    LOCATION = "eastus2" # Replace with the location of your compute cluster
>    ```
>

1. Zadajte nasledujúci príkaz na spustenie skriptu *setup_ml.py* a začatie procesu doladenia v Azure Machine Learning.

    ```python
    python setup_ml.py
    ```

1. V tomto cvičení ste úspešne doladili model Phi-3 pomocou Azure Machine Learning. Spustením skriptu *setup_ml.py* ste nastavili prostredie Azure Machine Learning a spustili proces doladenia definovaný v súbore *fine_tune.py*. Upozorňujeme, že doladenie môže trvať pomerne dlho. Po spustení `python setup_ml.py` command, you need to wait for the process to complete. You can monitor the status of the fine-tuning job by following the link provided in the terminal to the Azure Machine Learning portal.

    ![See finetuning job.](../../../../../../translated_images/02-02-see-finetuning-job.a28c8552f7b7bc088ccd67dd0c522f7fc1944048d3554bb1b24f95a1169ad538.sk.png)

### Deploy the fine-tuned model

To integrate the fine-tuned Phi-3 model with Prompt Flow, you need to deploy the model to make it accessible for real-time inference. This process involves registering the model, creating an online endpoint, and deploying the model.

#### Set the model name, endpoint name, and deployment name for deployment

1. Open *config.py* file.

1. Replace `AZURE_MODEL_NAME = "your_fine_tuned_model_name"` with the desired name for your model.

1. Replace `AZURE_ENDPOINT_NAME = "your_fine_tuned_model_endpoint_name"` with the desired name for your endpoint.

1. Replace `AZURE_DEPLOYMENT_NAME = "your_fine_tuned_model_deployment_name"` zadajte požadované meno pre vaše nasadenie.

#### Pridajte kód do súboru *deploy_model.py*

Spustením súboru *deploy_model.py* automatizujete celý proces nasadenia. Tento súbor zaregistruje model, vytvorí endpoint a vykoná nasadenie na základe nastavení v súbore config.py, ktorý obsahuje názov modelu, názov endpointu a názov nasadenia.

1. Otvorte súbor *deploy_model.py* vo Visual Studio Code.

1. Pridajte nasledujúci kód do *deploy_model.py*.

    ```python
    import logging
    from azure.identity import AzureCliCredential
    from azure.ai.ml import MLClient
    from azure.ai.ml.entities import Model, ProbeSettings, ManagedOnlineEndpoint, ManagedOnlineDeployment, IdentityConfiguration, ManagedIdentityConfiguration, OnlineRequestSettings
    from azure.ai.ml.constants import AssetTypes

    # Configuration imports
    from config import (
        AZURE_SUBSCRIPTION_ID,
        AZURE_RESOURCE_GROUP_NAME,
        AZURE_ML_WORKSPACE_NAME,
        AZURE_MANAGED_IDENTITY_RESOURCE_ID,
        AZURE_MANAGED_IDENTITY_CLIENT_ID,
        AZURE_MODEL_NAME,
        AZURE_ENDPOINT_NAME,
        AZURE_DEPLOYMENT_NAME
    )

    # Constants
    JOB_NAME = "your-job-name"
    COMPUTE_INSTANCE_TYPE = "Standard_E4s_v3"

    deployment_env_vars = {
        "SUBSCRIPTION_ID": AZURE_SUBSCRIPTION_ID,
        "RESOURCE_GROUP_NAME": AZURE_RESOURCE_GROUP_NAME,
        "UAI_CLIENT_ID": AZURE_MANAGED_IDENTITY_CLIENT_ID,
    }

    # Logging setup
    logging.basicConfig(
        format="%(asctime)s - %(levelname)s - %(name)s - %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        level=logging.DEBUG
    )
    logger = logging.getLogger(__name__)

    def get_ml_client():
        """Initialize and return the ML Client."""
        credential = AzureCliCredential()
        return MLClient(credential, AZURE_SUBSCRIPTION_ID, AZURE_RESOURCE_GROUP_NAME, AZURE_ML_WORKSPACE_NAME)

    def register_model(ml_client, model_name, job_name):
        """Register a new model."""
        model_path = f"azureml://jobs/{job_name}/outputs/artifacts/paths/model_output"
        logger.info(f"Registering model {model_name} from job {job_name} at path {model_path}.")
        run_model = Model(
            path=model_path,
            name=model_name,
            description="Model created from run.",
            type=AssetTypes.MLFLOW_MODEL,
        )
        model = ml_client.models.create_or_update(run_model)
        logger.info(f"Registered model ID: {model.id}")
        return model

    def delete_existing_endpoint(ml_client, endpoint_name):
        """Delete existing endpoint if it exists."""
        try:
            endpoint_result = ml_client.online_endpoints.get(name=endpoint_name)
            logger.info(f"Deleting existing endpoint {endpoint_name}.")
            ml_client.online_endpoints.begin_delete(name=endpoint_name).result()
            logger.info(f"Deleted existing endpoint {endpoint_name}.")
        except Exception as e:
            logger.info(f"No existing endpoint {endpoint_name} found to delete: {e}")

    def create_or_update_endpoint(ml_client, endpoint_name, description=""):
        """Create or update an endpoint."""
        delete_existing_endpoint(ml_client, endpoint_name)
        logger.info(f"Creating new endpoint {endpoint_name}.")
        endpoint = ManagedOnlineEndpoint(
            name=endpoint_name,
            description=description,
            identity=IdentityConfiguration(
                type="user_assigned",
                user_assigned_identities=[ManagedIdentityConfiguration(resource_id=AZURE_MANAGED_IDENTITY_RESOURCE_ID)]
            )
        )
        endpoint_result = ml_client.online_endpoints.begin_create_or_update(endpoint).result()
        logger.info(f"Created new endpoint {endpoint_name}.")
        return endpoint_result

    def create_or_update_deployment(ml_client, endpoint_name, deployment_name, model):
        """Create or update a deployment."""

        logger.info(f"Creating deployment {deployment_name} for endpoint {endpoint_name}.")
        deployment = ManagedOnlineDeployment(
            name=deployment_name,
            endpoint_name=endpoint_name,
            model=model.id,
            instance_type=COMPUTE_INSTANCE_TYPE,
            instance_count=1,
            environment_variables=deployment_env_vars,
            request_settings=OnlineRequestSettings(
                max_concurrent_requests_per_instance=3,
                request_timeout_ms=180000,
                max_queue_wait_ms=120000
            ),
            liveness_probe=ProbeSettings(
                failure_threshold=30,
                success_threshold=1,
                period=100,
                initial_delay=500,
            ),
            readiness_probe=ProbeSettings(
                failure_threshold=30,
                success_threshold=1,
                period=100,
                initial_delay=500,
            ),
        )
        deployment_result = ml_client.online_deployments.begin_create_or_update(deployment).result()
        logger.info(f"Created deployment {deployment.name} for endpoint {endpoint_name}.")
        return deployment_result

    def set_traffic_to_deployment(ml_client, endpoint_name, deployment_name):
        """Set traffic to the specified deployment."""
        try:
            # Fetch the current endpoint details
            endpoint = ml_client.online_endpoints.get(name=endpoint_name)
            
            # Log the current traffic allocation for debugging
            logger.info(f"Current traffic allocation: {endpoint.traffic}")
            
            # Set the traffic allocation for the deployment
            endpoint.traffic = {deployment_name: 100}
            
            # Update the endpoint with the new traffic allocation
            endpoint_poller = ml_client.online_endpoints.begin_create_or_update(endpoint)
            updated_endpoint = endpoint_poller.result()
            
            # Log the updated traffic allocation for debugging
            logger.info(f"Updated traffic allocation: {updated_endpoint.traffic}")
            logger.info(f"Set traffic to deployment {deployment_name} at endpoint {endpoint_name}.")
            return updated_endpoint
        except Exception as e:
            # Log any errors that occur during the process
            logger.error(f"Failed to set traffic to deployment: {e}")
            raise


    def main():
        ml_client = get_ml_client()

        registered_model = register_model(ml_client, AZURE_MODEL_NAME, JOB_NAME)
        logger.info(f"Registered model ID: {registered_model.id}")

        endpoint = create_or_update_endpoint(ml_client, AZURE_ENDPOINT_NAME, "Endpoint for finetuned Phi-3 model")
        logger.info(f"Endpoint {AZURE_ENDPOINT_NAME} is ready.")

        try:
            deployment = create_or_update_deployment(ml_client, AZURE_ENDPOINT_NAME, AZURE_DEPLOYMENT_NAME, registered_model)
            logger.info(f"Deployment {AZURE_DEPLOYMENT_NAME} is created for endpoint {AZURE_ENDPOINT_NAME}.")

            set_traffic_to_deployment(ml_client, AZURE_ENDPOINT_NAME, AZURE_DEPLOYMENT_NAME)
            logger.info(f"Traffic is set to deployment {AZURE_DEPLOYMENT_NAME} at endpoint {AZURE_ENDPOINT_NAME}.")
        except Exception as e:
            logger.error(f"Failed to create or update deployment: {e}")

    if __name__ == "__main__":
        main()

    ```

1. Vykonajte nasledujúce kroky na získanie `JOB_NAME`:

    - Navigate to Azure Machine Learning resource that you created.
    - Select **Studio web URL** to open the Azure Machine Learning workspace.
    - Select **Jobs** from the left side tab.
    - Select the experiment for fine-tuning. For example, *finetunephi*.
    - Select the job that you created.
    - Copy and paste your job Name into the `JOB_NAME = "your-job-name"` in *deploy_model.py* file.

1. Replace `COMPUTE_INSTANCE_TYPE` s vašimi konkrétnymi údajmi.

1. Zadajte nasledujúci príkaz na spustenie skriptu *deploy_model.py* a začatie procesu nasadenia v Azure Machine Learning.

    ```python
    python deploy_model.py
    ```

> [!WARNING]
> Aby ste predišli ďalším poplatkom na vašom účte, nezabudnite odstrániť vytvorený endpoint v Azure Machine Learning workspace.
>

#### Skontrolujte stav nasadenia v Azure Machine Learning Workspace

1. Navštívte [Azure ML Studio](https://ml.azure.com/home?wt.mc_id=studentamb_279723).

1. Prejdite do Azure Machine Learning workspace, ktorý ste vytvorili.

1. Vyberte **Studio web URL** na otvorenie Azure Machine Learning workspace.

1. Vyberte **Endpoints** v ľavom paneli.

    ![Vyberte endpoints.](../../../../../../translated_images/02-03-select-endpoints.a32f4eb2854cd54ee997f9bec0e842c3084bbc24bd693457b5c6b132fe966bf4.sk.png)

2. Vyberte endpoint, ktorý ste vytvorili.

    ![Vyberte vytvorený endpoint.](../../../../../../translated_images/02-04-select-endpoint-created.048b4f0f6479c1885b62711a151227a24408679be65dd1039cd2f64448ec5842.sk.png)

3. Na tejto stránke môžete spravovať endpointy vytvorené počas procesu nasadenia.

## Scenár 3: Integrácia s Prompt flow a chatovanie s vlastným modelom

### Integrácia vlastného modelu Phi-3 s Prompt flow

Po úspešnom nasadení doladeného modelu ho teraz môžete integrovať s Prompt flow, aby ste mohli svoj model používať v reálnych aplikáciách a realizovať rôzne interaktívne úlohy s vlastným modelom Phi-3.

#### Nastavte api key a endpoint uri doladeného modelu Phi-3

1. Prejdite do Azure Machine Learning workspace, ktorý ste vytvorili.
1. Vyberte **Endpoints** v ľavom paneli.
1. Vyberte endpoint, ktorý ste vytvorili.
1. Vyberte **Consume** v navigačnom menu.
1. Skopírujte a vložte **REST endpoint** do súboru *config.py*, pričom nahraďte `AZURE_ML_ENDPOINT = "your_fine_tuned_model_endpoint_uri"` with your **REST endpoint**.
1. Copy and paste your **Primary key** into the *config.py* file, replacing `AZURE_ML_API_KEY = "your_fine_tuned_model_api_key"` vaším **Primárnym kľúčom**.

    ![Skopírujte api key a endpoint uri.](../../../../../../translated_images/02-05-copy-apikey-endpoint.602de7450770e9984149dc7da7472bacafbf0e8447e2adb53896ad93b1dc7684.sk.png)

#### Pridajte kód do súboru *flow.dag.yml*

1. Otvorte súbor *flow.dag.yml* vo Visual Studio Code.

1. Pridajte nasledujúci kód do *flow.dag.yml*.

    ```yml
    inputs:
      input_data:
        type: string
        default: "Who founded Microsoft?"

    outputs:
      answer:
        type: string
        reference: ${integrate_with_promptflow.output}

    nodes:
    - name: integrate_with_promptflow
      type: python
      source:
        type: code
        path: integrate_with_promptflow.py
      inputs:
        input_data: ${inputs.input_data}
    ```

#### Pridajte kód do súboru *integrate_with_promptflow.py*

1. Otvorte súbor *integrate_with_promptflow.py* vo Visual Studio Code.

1. Pridajte nasledujúci kód do *integrate_with_promptflow.py*.

    ```python
    import logging
    import requests
    from promptflow.core import tool
    import asyncio
    import platform
    from config import (
        AZURE_ML_ENDPOINT,
        AZURE_ML_API_KEY
    )

    # Logging setup
    logging.basicConfig(
        format="%(asctime)s - %(levelname)s - %(name)s - %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        level=logging.DEBUG
    )
    logger = logging.getLogger(__name__)

    def query_azml_endpoint(input_data: list, endpoint_url: str, api_key: str) -> str:
        """
        Send a request to the Azure ML endpoint with the given input data.
        """
        headers = {
            "Content-Type": "application/json",
            "Authorization": f"Bearer {api_key}"
        }
        data = {
            "input_data": [input_data],
            "params": {
                "temperature": 0.7,
                "max_new_tokens": 128,
                "do_sample": True,
                "return_full_text": True
            }
        }
        try:
            response = requests.post(endpoint_url, json=data, headers=headers)
            response.raise_for_status()
            result = response.json()[0]
            logger.info("Successfully received response from Azure ML Endpoint.")
            return result
        except requests.exceptions.RequestException as e:
            logger.error(f"Error querying Azure ML Endpoint: {e}")
            raise

    def setup_asyncio_policy():
        """
        Setup asyncio event loop policy for Windows.
        """
        if platform.system() == 'Windows':
            asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())
            logger.info("Set Windows asyncio event loop policy.")

    @tool
    def my_python_tool(input_data: str) -> str:
        """
        Tool function to process input data and query the Azure ML endpoint.
        """
        setup_asyncio_policy()
        return query_azml_endpoint(input_data, AZURE_ML_ENDPOINT, AZURE_ML_API_KEY)

    ```

### Chatovanie s vlastným modelom

1. Zadajte nasledujúci príkaz na spustenie skriptu *deploy_model.py* a začatie procesu nasadenia v Azure Machine Learning.

    ```python
    pf flow serve --source ./ --port 8080 --host localhost
    ```

1. Tu je príklad výsledkov: Teraz môžete chatovať so svojím vlastným modelom Phi-3. Odporúča sa klásť otázky založené na dátach použitých na doladenie.

    ![Príklad Prompt flow.](../../../../../../translated_images/02-06-promptflow-example.023c07a4be8f02199e04eaf49f40ba24415da1be2274cbda9a7aa39776acd0bb.sk.png)

**Vyhlásenie o zodpovednosti**:  
Tento dokument bol preložený pomocou AI prekladateľskej služby [Co-op Translator](https://github.com/Azure/co-op-translator). Aj keď sa snažíme o presnosť, prosím, majte na pamäti, že automatické preklady môžu obsahovať chyby alebo nepresnosti. Originálny dokument v jeho pôvodnom jazyku by mal byť považovaný za autoritatívny zdroj. Pre kritické informácie sa odporúča profesionálny ľudský preklad. Nie sme zodpovední za akékoľvek nedorozumenia alebo nesprávne interpretácie vyplývajúce z použitia tohto prekladu.
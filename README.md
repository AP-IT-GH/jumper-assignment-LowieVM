# jumper-assignment-LowieVM

## Overzicht

Dit project implementeert een zelflerende agent in Unity met behulp van ML-Agents. De agent leert obstakels te ontwijken door erover te springen. Obstakels krijgen elke episode een andere snelheid. Daarnaast zijn er bepaalde obstakels die bonuspunten opleveren bij een botsing, terwijl andere obstakels vermeden moeten worden.

## Implementatie Details

### Unity Setup

- **Versie:** Unity 6 (6000.0.36f1)
- **Package:** ML-Agents geïnstalleerd via de Unity Package Manager

### Agent Eigenschappen

- **Observaties:**
  - Raycast detectie van de omgeving
  - Extra observaties:
    - Of de agent op de grond is (en dus kan springen)
    - Positie en snelheid van het obstakel/reward
    - Type object dat op de agent afkomt (obstacle of reward)
- **Acties:**
  - **DiscreteAction**: Springen (1) of niet springen (0)

### Reward Systeem

- Springen geeft een negatieve reward van **-0.01**
- Niet springen geeft een positieve reward van **0.005**
- In leven blijven levert **0.01** per stap op
- Succesvol over een obstakel springen: **+1.0**
- Drie obstakels na elkaar correct springen: **+0.5**
- Over een reward springen: **-0.5**
- Een reward pakken: **+0.5**
- Obstakel raken: **-1.0** en einde episode

Kleine aanpassingen aan deze reward-waardes beïnvloeden het leerproces sterk. Zo leidde het weglaten van de negatieve spring-reward ertoe dat de agent constant sprong, terwijl een te negatieve spring-reward ervoor zorgde dat de agent nauwelijks sprong.

## Installatie van ML-Agents in Python

Voor de training van de agent is een Python-omgeving nodig. Hiervoor is Anaconda gebruikt.

1. **Installeer Anaconda**: [Download hier](https://www.anaconda.com/download)
2. **Maak een nieuwe omgeving in Anaconda**:
   ```sh
   conda create --name mlagents python=3.9.18
   conda activate mlagents
   ```
3. **Installeer de ML-Agents package**:
   ```sh
   pip3 install torch~=1.7.1 -f https://download.pytorch.org/whl/torch_stable.html
   pip install protobuf==3.20.*
   python -m pip install mlagents==0.30.0
   ```

## Training van de Agent

Voor het trainen van de agent is een configuratiebestand vereist.

1. **Maak een YAML-configuratiebestand**:

   - Plaats het bestand `Agent.yaml` in de `config/` map van het Unity-project (de inhoud van dit bestand staat in de GitHub-repo).

2. **Start de training**:

   ```sh
   mlagents-learn config/Agent.yaml --force --run-id=Agent
   ```

   - Start vervolgens de Unity Editor en druk op **Play** om de training te laten beginnen.

## Training Resultaten

Hieronder staat een TensorBoard-figuur die het verloop van de training laat zien:

![tensorboard image](/tensorboard.png)

De blauwe lijn toont de training waarbij de agent extra observaties had, terwijl de oranje lijn een eerdere versie zonder deze observaties toont. Dankzij deze extra observaties leerde de agent veel sneller en efficiënter obstakels te vermijden.

## Conclusie

Door zorgvuldig de rewards en observaties af te stemmen, kon een efficiënte agent worden getraind. Belangrijke lessen waren:

- Balans in rewards: Te straffen of belonen beïnvloedt het gedrag sterk.
- Extra observaties: Bepaalde observaties waren cruciaal om de leercurve te verbeteren.

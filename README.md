# Prediction de temperature quotidienne — RNN et LSTM PyTorch, sans interface graphique

Projet Python local pour Windows / VS Code. Toutes les commandes fonctionnent dans le terminal.
Aucune interface Tkinter. PyTorch fonctionne sur CPU : pas besoin d'une carte graphique.

## Donnees
Telecharger le fichier **DailyDelhiClimateTrain.csv** sur :
https://www.kaggle.com/datasets/sumanthvrao/daily-climate-time-series-data

Placer le CSV dans `data/DailyDelhiClimateTrain.csv` (non inclus dans ce ZIP).
Colonnes obligatoires : `date,meantemp,humidity,wind_speed,meanpressure`.

## Installer avec PowerShell dans VS Code
```powershell
py -3 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```
Si l'activation PowerShell est interdite, utiliser directement ` .\.venv\Scripts\python.exe ` dans les commandes suivantes.
**PyTorch est sensiblement plus volumineux que NumPy ; l'installation peut etre longue.**

## Entrainer et evaluer
```powershell
python train.py
```
Essai rapide : `python train.py --epochs 3` (resultats uniquement pour verifier le fonctionnement).
Parametres : `--epochs 80 --hidden 16 --batch-size 32 --patience 12 --lr 0.001 --csv chemin/vers/fichier.csv`.

## Predire apres la derniere date du CSV
```powershell
python predict.py
python predict.py --model rnn_1var
```
`predict.py` charge un fichier `models/*.pt` genere par l'entrainement.

## Methodologie
- Fenetre de 14 jours, cible : temperature du jour suivant.
- 70 % entrainement, 15 % validation, 15 % test, dans l'ordre chronologique.
- Normalisation et plafonnement des valeurs extremes appris sur l'entrainement.
- Deux references : persistence (temperature precedente) et moyenne sur 14 jours.
- Quatre modeles : RNN 1 et 4 variables ; LSTM 1 et 4 variables.
- MSE pour entrainer, optimisation Adam, gradient clipping, early stopping sur validation.
- MAE et RMSE sur test, en degres Celsius.
- `results/metrics.csv` : scores, `results/predictions.csv` : predictions,
  `results/comparaison.png` : image du graphique (pas de fenetre interactive),
  `results/historique_*.csv` : pertes pendant les epoques.
- `models/*.pt` : modeles PyTorch sauvegardes.

Le dataset historique ne donne pas la temperature de demain reel par rapport a aujourd'hui.
Comparer objectivement les performances : LSTM peut etre moins performant que la persistence.
"# RNN_Projet_Temperature" 

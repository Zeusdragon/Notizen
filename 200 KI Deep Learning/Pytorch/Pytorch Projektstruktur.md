1. Datenquelle --> (Datenbank oder Messdateien) verarbeitung mit torch.tensor
2. Daten werden zu Trainingsdaten( **to.tensor** ) sowie Festlegung von x und y zum validieren (**X/Y**) sowie Testdaten (split)
3. Dataloader bauen Training,Test und Validation zusammenfassen
4. Model definieren dabei init class bauen sowie forward
	- Init werden die verschiedenen aktivierungslayers definiert (ReLU oder sigmoid etc) 
5. Modeltrainieren dabei Trainingsloop gibt eine loop mit bisherigen weights and biases durch validation macht eine loss accuracy um später damit anpassungen zu machen
6. Zum schluss evaluieren wie gut das model trainiert ist mit graph
7. mit test daten schauen wie gut das Trainierte Modell performt
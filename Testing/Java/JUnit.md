Framework per eseguire [[Unit test]] in [[Java]].
- Ogni metodo di test deve essere pubblico, non void, non statico e annotato con $@test$.
- Sorgente e test devono essere in due folder separati. Così quando si consegna il JAR si possono omettere i test.
- Ogni metodo di test ha quattro fasi:
	- Setup
		Si crea un'istanza del SUT.
	- Exercise
		Si chiama il metodo da testare.
	- Verify
		Si verifica che il metodo faccia cosa vogliamo.
	- Teardown
		Si fa pulizia di quello che ci è servito per testare, in modo che i successivi test non siano influenzati.
- Si possono eseguire metodi statici una volta e prima di ogni altro metodo annotando tali metodi con $@Beforeclass$ o dopo con $Afterclass$. Si possono eseguire metodi d'istanza ogni volta prima di un metodo di test annotando tali metodi con $@Before$ o dopo con $After$.
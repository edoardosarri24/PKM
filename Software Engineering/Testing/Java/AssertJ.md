Framework e libreria per eseguire [[Unit test]] in [[Java]].
Ci sono molti metodi statici, la cui firma è molto chiara, che possono essere chiamati a cascata; inoltre i messaggi di errore sono molto chiari.

Il tutto si basa sul metodo $assertThat$. Al metodo si passa un solo argomento, che è il soggetto del test; questo ritorna un oggetto su cui chiamare a cascata altri metodi. Il tipo di ritorno dipende dal tipo statico del soggetto.
## USO
- Scaricare il JAR dal [link](https://search.maven.org/artifact/org.assertj/assertj-core/3. 14.0/bundle).
- Creare una directory (Non source-foder) in cui mettere il jar scaricato.
- Dal menù contestuale del JAR usare $Add to Build Path$.
- Importare nel test case il metodo statico $import\ static\  org.assertj.core.api.Assertions.*;$.
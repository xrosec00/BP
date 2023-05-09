Model byl vytvořen ve verzi UPPAAL 4.1.26. Starší verze programu nemusí být kompatibilní.

Důležité parametry pro práci s modelem a jejich význam:

V globálních deklaracích "Declarations":

const int N - definuje velikost sady úloh, měla by vždy odpovídat právě aktivní sadě (např sada 2 má dvě úlohy, N = 2, pro většinu sad N = 3)

const int buffSize - definuje velikost fronty úloh, pro sady bez aperiodických úloh by mělo platit buffSize = N, pro sady s aperiodickou úlohou
	 se buffSize - N + 1 rovná počtu instancí aperiodické úlohy (pro buffSize = N jde tedy o 1 instanci úlohy)

každá sada má na řádku pod sebou zakomentovanou funkci task_queue = {...}, tento řádek nastavuje danou sadu jako aktivní, je třeba odkomentovat pro 
	 aktivní sadu a zakomentovat pro ostatní sady. původně bylo přiřazování řešeno přiřazením sady rovnou do task_queue, ale se sadami o různém
	počtu úloh toto zpoůsobovalo v programu chybu kvůli tomu, že velikost polí musí být v UPPAAL předem známá
u sad s méně úlohami je pak také potřeba zakomentovat úlohy u ostatních sad s id vyšším, než je velikost aktivní sady, toto je z důvodu přiřazení
	speciálního typu t_id úlohám, protože t_id je omezené od 0 do N-1

V systémových deklaracích:

Sched = Scheduler(int policy) - integer policy označuje žádaný řadící algortimus, jednotlivá čísla a k nim náležející algoritmy jsou uvedené v komentáři
Příklad: Sched = Scheduler(0) pro FIFO

Policy = x_model() - x je žádaný řadící algoritmus zapsaný svou zkratkou
Příklad: Policy = FIFO_model()

RR= RoundRobin_model(enabled, quantum) - bool enabled označuje, zda má algoritmus Round Robin být použit nebo ne, int quantum pak udává délku kvanta
RR= RoundRobin_model(true, 5) - Round Robin je zapnutý, délka kvanta je 5 časových jednotek

Pro využití pozorovatele (Monitor) je nutné přidat jeho instanci Mon do System, působí pak však chybu u queries typu A[], A<>, E<>, A<>,
	proto je lepší ho vždy odebrat, pokud jej daná query nepoužívá.
	Pozorovatel slouží primárně jen pro určení využití CPU a počítání počtu iterací, pro ostatní queries není potřeba. Pozorovatel nemá vliv 
	na běh programu, je pouze pasivní.
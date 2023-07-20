# Building My Genome By Vini-farmainbioinfo
Esse pipeline de montagem e avaliação da qualidade de genomas foi criado durante a confecção do Trabalho de Conclusão de Curso denominado "Análise genômica de bactérias multirresistentes isoladas de alimentos minimamente processados" do curso de Farmácia na Universidade Federal de Juiz de Fora (UFJF). O pipeline foi
foi pensado para ser uma maneira organizada, objetiva e de fácil entendimento contendo os passos necessários para montar e avaliar a qualidade de genomas provenientes do sequenciamento Illumina.


1 - Controle de qualidade visando a remoção das sequências de baixa qualidade é feito utilizando o software Trimmomatic (https://github.com/usadellab/Trimmomatic).

*Paramêtros: TRAILING: 10 LEADING: 10 SLIDINGWINDOW: 4:20 MINLEN: 50*

Se os dados são paired-end:

<pre><code> java -jar /usr/share/java/trimmomatic-0.39.jar PE input_1.fq.gz input_2.fq.gz reads_trimm/output_1_paired.fq.gz reads_trimm/ouput_1_unpaired.fq.gz reads_trimm/output_2_paired.fq.gz reads_trimm/output_2_unpaired.fq.gz TRAILING:10 LEADING:10 SLIDINGWINDOW:4:20 MINLEN:50 </code></pre>

Se os dados são single-end: 

<pre><code> java -jar /usr/share/java/trimmomatic-0.39.jar SE input_1.fq.gz reads_trimm/output_1_unpaired.fq.gz TRAILING:10 LEADING:10 SLIDINGWINDOW:4:20 MINLEN:50 </code></pre>

  
2 - Montagem do genoma usando o software SPAdes (https://github.com/ablab/spades)

*Paramêtros: -k 21,33,55,77,99,127 -t 4 &*

<pre><code> spades.py --careful -o ../outputdirectory -1 input_1_paired.fq.gz -2 input_2_paired.fq.gz -s input_1_unpaired.fq.gz -s input_2_unpaired.fq.gz -k 21,33,55,77,99,127 -t 4 & </code></pre>

3 - Verificação da qualidade da montagem usando o software QUAST. Este comando também irá gerar arquivos com a posição inicial e final do gene 16S (https://github.com/ablab/quast)

<pre><code> 
quast.py --glimmer --rna-finding /folder_results_SPAdes/contigs.fasta 
</code></pre>

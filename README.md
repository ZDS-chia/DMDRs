# DMDRs
###Identification of DMDRs
##Convert to .wig format
`./bin/towig --ratio_total 5,3 -i ./test_data/N01_chr10.txt -o N01_chr10 -n N01_chr10`
./bin/towig --ratio_total 5,3 -i ./test_data/N02_chr10.txt -o N02_chr10 -n N02_chr10
##Identification of methylation deficient region
./bin/pattern -i ./N01_chr10/N01_chr10_methy.wig -o N01_chr10_pattern/ -n N01_chr10_pattern
./bin/pattern -i ./N02_chr10/N02_chr10_methy.wig -o N02_chr10_pattern/ -n N02_chr10_pattern
##Filtration
cat ./N01_chr10_pattern/N01_chr10_pattern_UM.bed |awk '$2<$3 {print "chr"$0}' > ./N01_chr10_pattern/N01_chr10_pattern_UM-2.bed
cat ./N02_chr10_pattern/N02_chr10_pattern_UM.bed |awk '$2<$3 {print "chr"$0}' > ./N02_chr10_pattern/N02_chr10_pattern_UM-2.bed
##Subtract
bedtools subtract -a ./N01_chr10_pattern/N01_chr10_pattern_UM-2.bed -b ./N02_chr10_pattern/N02_chr10_pattern_UM-2.bed >N12.txt
##Compare differential methylation locis
bedtools intersect -a N12.txt -b ./test_data/N12-DML.bed -wa -wb > N12-region.txt
##Repeat the above steps to complete the difference area identification of all paired samples
##Merge(Taking two paired samples as an example)
cat ./test_data/N12-region.txt ./test_data/N34-region.txt > two_pairs_region.txt
sort -k1,1 -k2n,2 two_pairs_region.txt > two_pairs_region_sort.txt
bedtools merge -i two_pairs_region_sort.txt > merge_two_pairs_region.txt
##

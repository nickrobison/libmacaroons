fuzz-testing:
	rm -rf fuzz-testing
	mkdir -p fuzz-testing fuzz-testing/testcases-pre fuzz-testing/testcases fuzz-testing/out
	cmake -DCMAKE_C_COMPILER=afl-clang -B fuzz-testing/build -S ..
	make -C fuzz-testing/build fuzz_serializer -j6
	# Split the files into single lines, so we can fuzz one at a time.
	find unit -type f -name "serialization*" -not -name "*.sh" | xargs -I{} split -l 1 "{}" single
	find . -type f -name "single*" | xargs -I{} mv {} fuzz-testing/testcases-pre/
	afl-cmin -i fuzz-testing/testcases-pre -o fuzz-testing/testcases -- fuzz-testing/build/fuzz_serializer
	@echo "Execute: afl-fuzz -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer"

fuzzing-start:
	afl-fuzz -S fuzzer1 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -S fuzzer2 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -S fuzzer3 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -S fuzzer4 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -S fuzzer5 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -S fuzzer6 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -S fuzzer7 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer > /dev/null &
	afl-fuzz -M fuzzer0 -i fuzz-testing/testcases -o fuzz-testing/out fuzz-testing/build/fuzz_serializer

fuzzing-stop:
	-killall fuzzer
	-killall afl-fuzz

fuzzing-minimize:
	mkdir -p fuzz-testing/crashes-pre fuzz-testing/crashes
	find fuzz-testing/out/fuzzer*/crashes -type f -name "id*" | xargs -I{} cp "{}" fuzz-testing/crashes-pre
	afl-cmin -i fuzz-testing/crashes-pre -o fuzz-testing/crashes -- fuzz-testing/build/fuzz_serializer


.PHONY: fuzz-testing fuzzing-start fuzzing-stop fuzzing-minimize

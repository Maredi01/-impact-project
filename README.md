# -impact-project
Range numbers


package test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import org.testng.internal.collections.Pair;



public interface NumberRangeSummarizer {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		final Collection<Integer> collect = Arrays.asList(1,3,6,7,8,12,13,14,15,21,22,23,24,31);
		final Collection<Object> finalResult = collect.stream()
                // Firstly let's pair every number with a number it starts from in
                // given sequence
                .reduce(new ArrayList<Pair<Integer, Integer>>(), (result, number) -> {
                    if (result.isEmpty()) {
                        result.add(new Pair<>(number, number));
                        return result;
                    }

                    final Pair<Integer, Integer> previous = result.get(result.size() - 1);
                    if (previous.first() + 1 == number) {
                        result.add(new Pair<>(number, previous.second()));
                    } else {
                        result.add(new Pair<>(number, number));
                    } 
                    return result;
                }, (a, b) -> a)
                // Now let's group list of pair into a Map where key is a number 'from' and value is a list of values
                // in given sequence starting from 'from' number
                .stream()
                .collect(Collectors.groupingBy(Pair::second, Collectors.mapping(Pair::first, Collectors.toList())))
                // Finally let's sort entry set and convert into expected format
                .entrySet()
                .stream()
                .sorted(Comparator.comparing(Map.Entry::getKey))
                .map(e -> e.getValue().size() < 3 ?
                        e.getValue() :
                        Collections.singletonList(String.format("%d-%d", e.getValue().get(0), e.getValue().get(e.getValue().size() - 1))))
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
		
		System.out.println("Given :" + collect);
		System.out.println("OutPut :" +finalResult);
	}

}

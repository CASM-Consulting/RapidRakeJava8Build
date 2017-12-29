rapidrake
================

> A fast version of the Rapid Automatic Keyword Extraction (RAKE) algorithm

[![Linux Build Status](https://travis-ci.org/crew102/rapidrake-java.svg?branch=master)](https://travis-ci.org/crew102/rapidrake-java)

Installation
------------

Assuming you are using Maven, follow these two steps to use `rakidrake` in your Java project:

1. Include a dependency on `rapidrake` in your POM:

```xml
<dependency>
    <groupId>io.github.crew102</groupId>
    <artifactId>rapidrake</artifactId>
    <version>0.0.1</version>
</dependency>
```

2. Download the `opennlp` trained models for sentence detection and part-of-speech tagging. You can find these two models (trained on various languages) on [opennlp's model page](http://opennlp.sourceforge.net/models-1.5/). For example, you could use the English versions of the [sentence detection](http://opennlp.sourceforge.net/models-1.5/en-sent.bin) and [POS-tagger](http://opennlp.sourceforge.net/models-1.5/en-pos-maxent.bin) models.

Basic usage
------------

```java
import io.github.crew102.rapidrake.RakeAlgorithm;
import io.github.crew102.rapidrake.data.SmartWords;
import io.github.crew102.rapidrake.model.RakeParams;
import io.github.crew102.rapidrake.model.Result;

public class Example {

	public static void main(String[] args) throws java.io.IOException {
		
		// Create a parameter object. See the RakeParams docs for details.
		String[] stopWords = new SmartWords().getSmartWords(); 
		String[] stopPOS = {"VB", "VBD", "VBG", "VBN", "VBP", "VBZ"}; 
		int minWordChar = 1;
		boolean shouldStem = true;
		String phraseDelims = "[-,.?():;\"!/]";	
		RakeParams params = new RakeParams(stopWords, stopPOS, minWordChar, shouldStem, phraseDelims);
		
		// Create a RakeAlgorithm object
		String POStaggerURL = "model-bin/en-pos-maxent.bin"; // The URL of your POS tagging model
		String SentDetecURL = "model-bin/en-sent.bin"; // The URL of your sentence detection model
		RakeAlgorithm rakeAlg = new RakeAlgorithm(params, POStaggerURL, SentDetecURL);
		
		// Call the rake method
		String txt = "dogs are great, don't you agree? I love dogs, especially big dogs";
		Result result = rakeAlg.rake(txt);
		
		// Print the result
		System.out.println(result.distinct());
		
	}
}

// [dogs (1.33), great (1), big dogs (3.33)]
```

Learning more
------------

You can learn more about how RAKE works and the various parameters you can set by visiting `slowraker`'s [website](https://crew102.github.io/slowraker/index.html).
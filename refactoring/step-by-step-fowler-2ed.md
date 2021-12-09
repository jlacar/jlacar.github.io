# Step by Step: Martin Fowler, Refactoring 2ed

This is a blow-by-blow analysis of Martin Fowler's first refactoring example in the second edition of his 
_Refactoring_ book.

This is based off of Emily Bache's snapshot tests.

He doesn't explicitly say it but first, run the tests.

%H - heuristic?
%A - analysis

## Decomposing the `statement` function

%H - when you see a long function, mentally identify points that separate different parts of the overall behavior. 
Then break them apart. This will give you a shorter function.

%A - The smell: "Long Function"; The cue: Visual; The pain: Temporal-Cognitive - it takes too long to read and 
understand the function

This is debt because it slows us down. Every time we come to this function, we pay interest on this debt.

So the first order of business is to decompose (aren't we going to compose it though?) the function? There's just 
too much implementation detail noise in this function. We need to raise the signal-to-noise ratio.

Response - scan the function and identify points that separate different parts of the overall behavior.

### Move #1: Extract Function (Local)

Context: You have a long function that appears to have a few different parts doing different things

Rationale: 


Refactoring:

1. Run tests
2. Tell the story - understand what we're trying to do
3. Scan the code - sense cues: visual, auditory, temporal
4. Separate parts that have different intents
5. clarify intent
6. 


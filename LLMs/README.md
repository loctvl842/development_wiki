# LLM (Large Language Model)

## Context Size

`Context Size` is the number of tokens that the model uses to predict the next token. For example, if the context size is 4, the model uses the previous 4 tokens to predict the next token.

A model is trained with a specific context size. For example, GPT-2 has a context size of 1024, GPT-3 has a context size of 2048, and so on.

## Temperature

`Temperature` is a hyperparameter that controls the randomness of the predictions. A lower temperature value will make the model more deterministic, while a higher temperature value will make the model more random.

LLM will try to guess the next possible token. For example if the temperature is 0. The next token is randomly picked from a small dictionary. If the temperature is 1, the next token is picked from the a larger dictionary.

## Max Tokens

`Max Tokens` is a hyperparameter that controls the maximum number of tokens that the model will generate. A larger max tokens value will result in more diverse and potentially longer generated text.

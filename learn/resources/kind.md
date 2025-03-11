# Kind

Resources are of specific kind. An apple resource <kbd>🍏</kbd> is different from a banana resource <kbd>🍌</kbd>. The resource kind determines the resource fungibility and is computed as the hash of its [logic](logic.md) and [label](state.md#label).

$$
\texttt{kind} := h_\texttt{kind}(\texttt{logic},\,\texttt{label})
$$

The kind is used to check if transactions are [balanced](../transactions/delta.md) and is a requirement by the [resource machine](../page/) for a transaction to be executable.

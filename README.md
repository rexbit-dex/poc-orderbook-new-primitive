## 🦖 RexBit Exchange - POC of Orderbook MPC 🔎

## ◻ Initial description


This Order Book Proof of Concept (PoC) adopts a pragmatic approach, utilizing Secure Multi-Party Computation (MPC) to preserve order privacy until execution. 

By leveraging cryptographic techniques like secret sharing and masked input computation, the PoC strikes a balance between security and efficiency. Secret sharing distributes sensitive information across participating nodes, ensuring no single node has access to complete order data. Masked input computation allows arithmetic operations on encrypted data without decryption, preserving confidentiality.

The order book logic is implemented using MPC techniques to guarantee result correctness and input privacy while maintaining efficiency in this case off-chain.

> While this PoC utilizes MPC for practical demonstration purposes, future implementations will leverage a novel cryptographic primitive specifically designed for efficient order book operations. This primitive aims to achieve an optimal balance between security, privacy, and performance, addressing the computational limitations of traditional MPC protocols.
------------------------------------------------------------------------------------------------------------------------------------------------------

### ⚙ How It Works


1. **Initialization**: Three nodes initialize the MPC protocol, generating cryptographic key pairs. Secure channels are established, and shared parameters are agreed upon, including the finite field used for computations and the degree of the secret-sharing polynomial.
   
   ```python
   >>> nodes = [node(), node(), node()]
    ```

2. **Preprocessing**: Simulate preprocessing to establish a shared arithmetic circuit representing the order book logic. This involves encoding price levels and order quantities as finite field elements and constructing the circuit for secure comparison and aggregation operations.
   
   ```python
   >>> preprocess(nodes, prices=16)
   ```

3. **Order Submission**: Traders submit requests for ask and bid orders, including price and quantity information. Each node locally generates masks for these values using cryptographic techniques, preserving privacy while enabling subsequent computation.
   
   ```python
   >>> request_ask = request.ask()
   >>> request_bid = request.bid()
   ```

4. **Mask Generation**: Upon receiving order requests, each node independently generates random masks for the price and quantity values. These masks are shared among the nodes using a secure aggregation protocol, ensuring that the original order information remains hidden while enabling subsequent computation on the masked values.
   
   ```python
   >>> masks_ask = [node.masks(request_ask) for node in nodes]
   >>> masks_bid = [node.masks(request_bid) for node in nodes]
   ```

5. **Order Masking**: Traders combine their order requests with the received masks to create masked order representations. These masked orders obfuscate the original price and quantity while retaining the necessary information for subsequent matching and execution within the MPC protocol.
   
   ```python
   >>> order_ask = order(masks_ask, 3)
   >>> order_bid = order(masks_bid, 8)
   ```

6. **Broadcasting Orders**: Each node independently collects and validates the received masked orders, ensuring consistency and integrity before proceeding with the matching process.

7. **Outcome Determination**: Having received and validated the masked orders, each node utilizes the shared arithmetic circuit to independently compute its share of the overall outcome. This involves evaluating the circuit on the masked input values using secure multi-party computation techniques, ensuring that no individual node has access to the complete order information.
   
   ```python
    >>> shares = [node.outcome(order_ask, order_bid) for node in nodes]
    ```

8. **Reconstruction**: The workflow operator collects the outcome shares from each node and reconstructs the final outcome using a secure aggregation protocol. This involves combining the shares and removing the masks to reveal either a successful trade execution with the bid-ask spread or a null outcome if no match is found.
   
   ```python
    >>> reveal(shares)
    range(3, 8)
    >>> min(reveal(shares))
    3
    >>> max(reveal(shares))
    8
    ```
------------------------------------------------------------------------------------------------------------------------------------------------------
### 🔎 Learn More
- [A Pragmatic Introduction to Secure Multi-Party Computation](https://securecomputation.org/)
- [Finite Field](https://mathworld.wolfram.com/FiniteField.html)

------------------------------------------------------------------------------------------------------------------------------------------------------
### 📰 Contact
- **Website:** [Website](https://www.rexbit.exchange)
- **Email:**  [Contact Us](mailto:contact@rexbit.exchange)

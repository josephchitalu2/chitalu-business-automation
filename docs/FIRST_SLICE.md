# First delivery: customer credit and repayment

This is the acceptance plan for the first implementation milestone. The
[API proposal](API.md) describes the intended routes; this repo does not serve
those routes yet.

## Walkthrough

Use a fictional customer and an authorized operator in a test database. Store
amounts as integer ngwee; display amounts as kwacha.

| Step | Expected result |
| --- | --- |
| Record an authorized K2,000.00 credit sale | The customer owes K2,000.00. No cash enters the till from the unpaid sale. |
| Record a K750.00 repayment | The statement shows the sale, the repayment and K1,250.00 still due. |
| Retry the same repayment with the same request key | Return the existing result. Keep one repayment and the K1,250.00 balance. |
| Reuse the key with a different amount | Reject the conflicting request. Keep the ledger unchanged. |
| Record the remaining K1,250.00 | The statement shows a zero balance. The customer can see both repayments. |

Write the payment, any applicable cash movement and the audit record within one
transaction. The statement balance is derived from ledger entries. Only the cash
portion of a payment contributes to the drawer; a non-cash repayment still reduces
the customer balance.

## Evidence before calling the slice complete

1. Test the walkthrough from a fresh database, including zero or negative amounts.
2. Verify an unauthorized write leaves the ledger and audit state unchanged.
3. Force a failure during the repayment write and verify the transaction rolls back.
4. Run the same request key twice and verify there is one repayment and no duplicate
   drawer movement.
5. Record the runtime, setup command and test command in the [verification guide](TESTING.md).

Extend the same workflow to cash closing and expense approval only after the
credit and repayment rules are stable. See the [delivery plan](DELIVERY_PLAN.md)
for the remaining work.

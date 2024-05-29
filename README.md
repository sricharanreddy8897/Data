
SELECT fulfillment_id
FROM custevents
WHERE event_type = 7
  AND fulfillment_id NOT IN (
    SELECT fulfillment_id
    FROM custevents
    WHERE event_type IN (0, 1)
  );

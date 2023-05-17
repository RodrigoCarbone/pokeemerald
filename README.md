# Auto Pickup

Tutorial on how to apply changes using pokeemerald decomp so that items picked up by the ability Pickup go directly to the player's bag.

In [src/battle_script_commands.c](https://github.com/pret/pokeemerald/blob/master/src/battle_script_commands.c) , before the declaration of the function cmd_pickup around line 9553 you need to add the following function

```c
static void AutoPickup(struct Pokemon *mon) {
    u16 item = GetMonData(mon, MON_DATA_HELD_ITEM);
    if (AddBagItem(item, 1)) {
        item = ITEM_NONE;
        SetMonData(mon, MON_DATA_HELD_ITEM, &item);
    }
}

```

Then inside the function cmd_pickup you need to call the function that we just added after the for that starts around line 9610

```diff

    for (j = 0; j < (int)ARRAY_COUNT(sPickupProbabilities); j++)
    {
        if (sPickupProbabilities[j] > rand)
        {
            SetMonData(&gPlayerParty[i], MON_DATA_HELD_ITEM, &sPickupItems[lvlDivBy10 + j]);
            break;
        }
        else if (rand == 99 || rand == 98)
        {
            SetMonData(&gPlayerParty[i], MON_DATA_HELD_ITEM, &sRarePickupItems[lvlDivBy10 + (99 - rand)]);
            break;
        }
    }
+    AutoPickup(&gPlayerParty[i]);
}

```

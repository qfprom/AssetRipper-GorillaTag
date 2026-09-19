# AssetRipper (Gorilla Tag Post 2025 fork)

Quick heads up: this is a narrow, one-purpose fork. It's for ripping Gorilla Tag builds from late 2025 onward, nothing else.

Sometime around then, Another Axiom started slapping `[SerializeReference]` on fields that shouldn't have it — mainly plain `UnityEngine.Object`-derived arrays like `AudioSource[]`. Unity doesn't actually support that as a real polymorphic managed reference, so its serializer just falls back to writing the field as an ordinary PPtr array. Stock AssetRipper wasn't expecting that: it assumed any `[SerializeReference]` field always has real managed-reference registry data behind it, threw a `NotSupportedException` on the resulting format tag, and as a side effect dropped every single field on the containing MonoBehaviour, not just the one that confused it. `VRRig` was the big one that kept coming out completely empty because of this.

The fix lives in `FieldSerializer.IsUnityEngineObjectSerializeReference` and a small fallback in `ManagedReferenceRegistryReader` — it catches that specific misuse and reads the field as a normal PPtr instead of routing it through the managed reference registry.

If you're not ripping something from this Gorilla Tag lineage, you don't want this fork — grab upstream [AssetRipper/AssetRipper](https://github.com/AssetRipper/AssetRipper) instead. This one doesn't carry any other changes and isn't kept up as a general-purpose tool.

AssetRipper is a powerful tool for analyzing Unity game files. For example, it enables game developers to:

* Find dependency assets that were accidentally included in their game
* Convert their assets into the native Unity engine format
* Identify code that could not be inlined or trimmed from their build
* Find broken asset references that would cause issues in their game

AssetRipper supports Unity versions from `3.5.0` to `6000.4.X`. However, support quality may vary slightly for different Unity versions.

## Legal Disclaimers

* AssetRipper is licensed under the [GNU General Public License v3.0](LICENSE.md).
* AssetRipper is not sponsored by or affiliated with Unity Technologies or its affiliates.
* "Unity" is a registered trademark of Unity Technologies or its affiliates in the U.S. and elsewhere.
